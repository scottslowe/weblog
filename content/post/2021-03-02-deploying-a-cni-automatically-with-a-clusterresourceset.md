---
author: slowe
categories: Tutorial
comments: true
date: 2021-03-02T10:00:00-07:00
tags:
- AWS
- CAPI
- Kubernetes
title: Deploying a CNI Automatically with a ClusterResourceSet
url: /2021/03/02/deploying-a-cni-automatically-with-a-clusterresourceset/
---

Not too long ago I hosted [an episode of TGIK8s][link-3], where I explored some features of [Cluster API][link-4]. One of the features I explored on the show was [ClusterResourceSet][link-5], an experimental feature that allows users to automatically install additional components onto workload clusters when the workload clusters are provisioned. In this post, I'll show how to deploy a CNI plugin automatically using a ClusterResourceSet.<!--more-->

A lot of this post is inspired by a similar post on [installing Calico using a ClusterResourceSet][link-1]. Although that post is for vSphere and this one focuses on AWS, much of the infrastructure differences are abstracted away by Kubernetes and Cluster API.

At a high level, using ClusterResourceSet to install a CNI plugin automatically looks like this:

1. Make sure experimental features are enabled on your CAPI management cluster.
2. Create a ConfigMap that contains the information to deploy the CNI plugin.
3. Create a ClusterResourceSet that references the ConfigMap.
4. Deploy one or more workload clusters that match the cluster selector specified in the ClusterResourceSet.

The sections below describe each of these steps in more detail.

## Enabling Experimental Features

The _preferred_ way to enable experimental features on your management cluster is to use a setting in the `clusterctl` configuration file or its environment variable equivalent before you initialize the management cluster. Specifically, putting `EXP_CLUSTER_RESOURCE_SET: "true"` in the `clusterctl` configuration file or using `export EXP_CLUSTER_RESOURCE_SET=true` before initializing the management cluster with `clusterctl init` will enable the ClusterResourceSet functionality.

But what if your management cluster has already been initialized? It is possible to enable the functionality by editing a couple of the CAPI-related Deployments on your management cluster. Specifically, you'll need to edit the following Deployments:

* The "capi-controller-manager" Deployment in the "capi-system" namespace
* The "capi-controller-manager" Deployment in the "capi-webhook-system" namespace

In both cases, the edit is the same: you'll want to edit the `--featureGates` parameter to specify "true" for ClusterResourceSet. For example, before editing the "capi-controller-manager" Deployment in the "capi-system" namespace, running `kubectl -n capi-system get deployment capi-controller-manager -o yaml` would show this for the command line parameters for the "manager" container:

```yaml
- args:
  - --metrics-addr=127.0.0.1:8080
  - --enable-leader-election
  - --feature-gates=MachinePool=false,ClusterResourceSet=false
  command:
  - /manager
```

After editing, it should look like this:

```yaml
- args:
  - --metrics-addr=127.0.0.1:8080
  - --enable-leader-election
  - --feature-gates=MachinePool=false,ClusterResourceSet=true
  command:
  - /manager
```

Editing the Deployments will cause Kubernetes to automatically start new versions of the containers with the new command-line flags.

It's worth noting that the preferred way is to enable the experimental feature through the use of a `clusterctl` configuration file or the appropriate environment variable _before_ initializing your management cluster with `clusterctl init`.

Once the functionality is enabled, then you're ready to start creating the various components needed to use ClusterResourceSets. The first component you'll need to create is a ConfigMap.

## Create the ConfigMap for the CNI Plugin

Most CNI plugins provide a YAML manifest that will define the CustomResourceDefinitions (CRDs), controllers, and Pods/DaemonSets/Deployments that are necessary for the CNI plugin to function correctly. To enable a ClusterResourceSet to install the CNI plugin for you when provisioning a workload cluster, you'll need to take that installation manifest and place it into a ConfigMap on the management cluster. The ClusterResourceSet, which you'll create in the next section, will then reference this ConfigMap.

To create a ConfigMap that contains the YAML manifest to install Calico, you'd first download the desired version of the Calico manifest (we'll assume you call the downloaded manifest `calico.yaml`) and then run this command against the management cluster:

    kubectl create configmap calico-crs-configmap --from-file=calico.yaml

Make a note of the name you use (this example calls the ConfigMap "calico-crs-configmap"), as you'll need it in the next step when you create the ClusterResourceSet itself.

## Create the ClusterResourceSet

Next up is creating the ClusterResourceSet itself. Here's an example ClusterResourceSet that could be used to install a CNI plugin onto (or into) a workload cluster:

```yaml
---
apiVersion: addons.cluster.x-k8s.io/v1alpha3
kind: ClusterResourceSet
metadata:
  name: calico-crs
  namespace: default
spec:
  clusterSelector:
    matchLabels:
      cni: calico 
  resources:
  - kind: ConfigMap
    name: calico-crs-configmap
```

The two key things to note here are the `clusterSelector` and the `resources` fields. The `clusterSelector` field controls how Cluster API will match this ClusterResourceSet against one or more workload clusters. In this case, I'm using a `matchLabels` approach where the ClusterResourceSet will be applied to all workload clusters that have the `cni: calico` label present. If the label isn't present, the ClusterResourceSet won't apply to that workload cluster.

The `resources` field references the ConfigMap created in the previous step, which in turn contains the manifest for installing the CNI plugin. Note that a ClusterResourceSet can contain multiple resources; only a single resource is specified in this example. If you do specify multiple resources, keep in mind that all specified resources in the ClusterResourceSet will be applied to all workload clusters that match the cluster selector property. If you need more granularity/flexiblity, use separate ClusterResourceSets for each resource.

The ClusterResourceSet is defined on the management cluster, so once you've created the YAML manifest you'd use `kubectl apply` to apply it against the management cluster:

    kubectl apply -f calico-crs.yaml

All the setup is now completed, and you're ready to use the ClusterResourceSet with your workload cluster(s).

## Deploy a Workload Cluster

With the appropriate resources in place (in the form of ConfigMaps that encapsulate the desired YAML manifests) and the ClusterResourceSet defined, you're ready to deploy a workload cluster and have the ClusterResourceSet automatically install the specified resources---in this case, the CNI plugin.

Use `clusterctl config cluster` to generate the YAML manifest for a workload cluster, then edit the resulting output to include the "cni: calico" label on the Cluster object, like this:

```yaml
---
apiVersion: cluster.x-k8s.io/v1alpha3
kind: Cluster
metadata:
  name: workload-cluster-1
  namespace: default
  labels:
    cni: calico
```

Apply the workload cluster manifest using `kubectl apply -f <filename.yaml>`, and then sit back and watch Cluster API go to work. After a few minutes (it depends on your provider and your specific configuration), you should see the workload cluster nodes (which you can check after you grab the Kubeconfig with `clusterctl get kubeconfig`) go to "Ready" status with no further intervention on your part---meaning that the CNI plugin was installed successfully!

## Additional Resources

In learning about ClusterResourceSets, I found reading the [ClusterResourceSet CAEP][link-2] to be helpful.

I hope this post is useful. If you have any questions, comments, or suggestions for improvement, I'd love to hear from you. You can find me on [the Kubernetes Slack instance][link-6], or contact [me on Twitter][link-7]. Thanks!

[link-1]: https://samperrin.com/posts/cluster-api-vsphere-using-clusterresourceset-to-install-calico-cni/
[link-2]: https://github.com/kubernetes-sigs/cluster-api/blob/master/docs/proposals/20200220-cluster-resource-set.md
[link-3]: https://tgik.io/143
[link-4]: https://cluster-api.sigs.k8s.io
[link-5]: https://cluster-api.sigs.k8s.io/tasks/experimental-features/cluster-resource-set.html
[link-6]: https://kubernetes.slack.com
[link-7]: https://twitter.com/scott_lowe
