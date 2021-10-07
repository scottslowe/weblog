---
author: slowe
categories: Tutorial
comments: true
date: 2021-10-07T10:00:00-06:00
tags:
- CAPI
- Cilium
- Kubernetes
title: Installing Cilium via a ClusterResourceSet
url: /2021/10/07/installing-cilium-via-clusterresourceset/
---

In this post, I'm going to walk you through how to install [Cilium][link-1] onto a [Cluster API][link-2]-managed workload cluster using a ClusterResourceSet. It's reasonable to consider this post a follow-up to my earlier post that walked you through [using a ClusterResourceSet to install Calico][xref-1]. There's no need to read the earlier post, though, as this post includes all the information (or links to the information) you need. Ready? Let's jump in!<!--more-->

## Prerequisites

If you aren't already familiar with Cluster API---hereafter just referred to as CAPI---then I would encourage you to read [my introduction to Cluster API post][xref-2]. Although it is a bit dated (it was written in the very early days of the project, which [recently released version 1.0][link-3]). Some of the commands referenced in that post have changed, but the underlying concepts remain valid. If you're not familiar with Cilium, check out [their introduction to the project][link-4] for more information. Finally, if you're not familiar at all with the idea of ClusterResourceSets, you can read my earlier post or check out [the ClusterResourceSet CAEP document][link-5].

## Installing Cilium via a ClusterResourceSet

If you want to install Cilium via a ClusterResourceSet, the process looks something like this:

1. Create a ConfigMap with the instructions for installing Cilium.
2. Create a ClusterResourceSet that references the ConfigMap.
3. Profit (when you deploy matching workload clusters).

Let's look at these steps in a bit more detail.

### Creating the Installation ConfigMap

The [Cilium docs][link-4] generally recommend the use of the `cilium` CLI tool to install Cilium. The reasoning behind this is, as I understand it, that the `cilium` CLI tool can interrogate the Kubernetes cluster to gather information and then attempt to pick the best configuration options for you. Using `helm` is also another option recommended by the docs. For our purposes, however, neither of those approaches will work---using a ClusterResourceSet means you need to be able to supply YAML manifests.

Fortunately, the fact that Cilium supports [Helm][link-7] gives us a path forward via the use of `helm template` to render the templates locally. As per the docs on `helm template`, there are some caveats/considerations, but this was the only way I found to create YAML manifests for installing Cilium.

So, the first step to creating the ConfigMap you need is to set up the Helm repository:

    helm repo add cilium https://helm.cilium.io

Then render the templates locally:

    helm template cilium cilium/cilium --version 1.10.4 \
    --namespace kube-system > cilium-1.10.4.yaml

You may need to specify additional options/values as needed in the above command in order to accommodate your specific environment or requirements, of course.

Once you have the templates rendered, then create the ConfigMap that the ClusterResourceSet needs:

    kubectl create configmap cilium-crs-cm --from-file=cilium-1.10.4.yaml

This ConfigMap should be created on the appropriate CAPI management cluster, so ensure your Kubernetes context is set correctly.

### Creating the ClusterResourceSet

Now you're ready to create the ClusterResourceSet. Here's an example you could use as a starting point:

```yaml
---
apiVersion: addons.cluster.x-k8s.io/v1alpha4
kind: ClusterResourceSet
metadata:
  name: cilium-crs
  namespace: default
spec:
  clusterSelector:
    matchLabels:
      cni: cilium 
  resources:
  - kind: ConfigMap
    name: cilium-crs-cm
```

You can see that the ClusterResourceSet references the ConfigMap, which in turn contains the YAML to install Cilium (and that's the YAML applied against matching workload clusters).

### Deploy a Matching Workload Cluster

What determines a matching workload cluster? The `clusterSelector` portion of the ClusterResourceSet. In the example above, the ClusterResourceSet's `clusterSelector` specifies that workload clusters should have the `cni: cilium` label attached.

The label should be part of CAPI's `Cluster` object, like this:

```yaml
---
apiVersion: cluster.x-k8s.io/v1alpha4
kind: Cluster
metadata:
  name: cilium-cluster
  namespace: cilium-test
  labels:
    cni: cilium
```

When CAPI creates a workload cluster with that label and value, as shown in the example CAPI manifest above, then the ClusterResourceSet will automatically apply the contents of the ConfigMap against the cluster after it has been fully provisioned. The result: Cilium gets installed automatically on the new workload cluster!

I hope this post is useful. If you have any questions or any feedback, I'd love to hear from you! Feel free to find [me on Twitter][link-7], or connect with me on [the Kubernetes Slack instance][link-8]. For more Cilium information and assistance, I'd encourage you to check out [the Cilium Slack community][link-9].

[link-1]: https://cilium.io/
[link-2]: https://cluster-api.sigs.k8s.io/
[link-3]: https://www.cncf.io/blog/2021/10/06/kubernetes-cluster-api-reaches-production-readiness-with-version-1-0/
[link-4]: https://docs.cilium.io/en/v1.10/intro/
[link-5]: https://github.com/kubernetes-sigs/cluster-api/blob/main/docs/proposals/20200220-cluster-resource-set.md
[link-6]: https://helm.sh
[link-7]: https://twitter.com/scott_lowe
[link-8]: https://kubernetes.slack.com
[link-9]: http://cilium.slack.com
[xref-1]: {{< relref "2021-03-02-deploying-a-cni-automatically-with-a-clusterresourceset.md" >}}
[xref-2]: {{< relref "2019-08-26-an-introduction-to-kubernetes-cluster-api.md" >}}
