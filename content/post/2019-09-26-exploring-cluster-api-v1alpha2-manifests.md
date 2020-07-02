---
author: slowe
categories: Explanation
comments: true
date: 2019-09-26T12:00:00Z
tags:
- Kubernetes
- CAPI
- YAML
- AWS
title: Exploring Cluster API v1alpha2 Manifests
url: /2019/09/26/exploring-cluster-api-v1alpha2-manifests
---

The [Kubernetes][link-1] community recently released v1alpha2 of Cluster API (a monumental effort, congrats to everyone involved!), and with it comes a number of fairly significant changes. Aside from [the new Quick Start][link-2], there isn't (yet) a great deal of documentation on Cluster API (hereafter just called CAPI) v1alpha2, so in this post I'd like to explore the structure of the CAPI v1alpha2 YAML manifests, along with links back to the files that define the fields for the manifests. I'll focus on the CAPI provider for AWS (affectionately known as CAPA).<!--more-->

As a general note, any links back to the source code on GitHub will reference the v0.2.1 release for CAPI and the v0.4.0 release for CAPA, which are the first v1apha2 releases for these projects.

Let's start with looking at a YAML manifest to define a Cluster in CAPA (this is taken directly from the Quick Start):

```yaml
apiVersion: cluster.x-k8s.io/v1alpha2
kind: Cluster
metadata:
  name: capi-quickstart
spec:
  clusterNetwork:
    pods:
      cidrBlocks: ["192.168.0.0/16"]
  infrastructureRef:
    apiVersion: infrastructure.cluster.x-k8s.io/v1alpha2
    kind: AWSCluster
    name: capi-quickstart
---
apiVersion: infrastructure.cluster.x-k8s.io/v1alpha2
kind: AWSCluster
metadata:
  name: capi-quickstart
spec:
  region: us-east-1
  sshKeyName: default
```

Right off the bat, I'll draw your attention to the separate Cluster and AWSCluster objects. CAPI v1alpha2 begins to more cleanly separate CAPI from the various CAPI providers (CAPA, in this case) by providing "generic" CAPI objects that map to provider-specific objects (like AWSCluster for CAPA). The link between the two is found in the `infrastructureRef` field, which references the AWSCluster object by name.

Astute readers may also note that the API group has changed from "cluster.k8s.io" in v1alpha1 to "cluster.x-k8s.io" in v1alpha2.

More details on these objects and the fields users can use to define them can be found in [`cluster_types.go`][link-3] (for the CAPI Cluster object) and in [`awscluster_types.go`][link-4] (for the AWSCluster object in CAPA). In particular, look for the definitions of the `ClusterSpec` and `AWSClusterSpec` structs (data structures).

The `AWSClusterSpec` struct still supports a `NetworkSpec` struct that users can use to influence how CAPA instantiates infrastructure, so the techniques I outlined [here][xref-1] (for creating highly available clusters) and [here][xref-2] (for consuming pre-existing AWS infrastructure) should still work with v1alpha2. (I'll update this post once I've had a chance to fully test.)

Now, let's look at some v1alpha2 YAML for creating a node in a cluster (in CAPI parlance, a node in a cluster is a Machine; as before, this example is taken from the Quick Start):

```yaml
apiVersion: cluster.x-k8s.io/v1alpha2
kind: Machine
metadata:
  name: capi-quickstart-controlplane-0
  labels:
    cluster.x-k8s.io/control-plane: "true"
    cluster.x-k8s.io/cluster-name: "capi-quickstart"
spec:
  version: v1.15.3
  bootstrap:
    configRef:
      apiVersion: bootstrap.cluster.x-k8s.io/v1alpha2
      kind: KubeadmConfig
      name: capi-quickstart-controlplane-0
  infrastructureRef:
    apiVersion: infrastructure.cluster.x-k8s.io/v1alpha2
    kind: AWSMachine
    name: capi-quickstart-controlplane-0
---
apiVersion: infrastructure.cluster.x-k8s.io/v1alpha2
kind: AWSMachine
metadata:
  name: capi-quickstart-controlplane-0
spec:
  instanceType: t3.large
  iamInstanceProfile: "controllers.cluster-api-provider-aws.sigs.k8s.io"
  sshKeyName: default
---
apiVersion: bootstrap.cluster.x-k8s.io/v1alpha2
kind: KubeadmConfig
metadata:
  name: capi-quickstart-controlplane-0
spec:
  initConfiguration:
    nodeRegistration:
      name: '{{ ds.meta_data.hostname }}'
      kubeletExtraArgs:
        cloud-provider: aws
  clusterConfiguration:
    apiServer:
      extraArgs:
        cloud-provider: aws
    controllerManager:
      extraArgs:
        cloud-provider: aws
```

In addition to the split into a "generic" CAPI object (a Machine) and a provider-specific object (an AWSMachine) like shown above with Cluster and AWSCluster, for nodes the CAPI v1alpha2 release also brings a KubeadmConfig object. This new object allows users to customize the `kubeadm` configuration used by CAPI to configure nodes when bringing up the cluster. In this particular example, the KubeadmConfig object is enabling the AWS cloud provider (see [this post][xref-3] for more details and links to other related posts regarding the AWS cloud provider).

Readers interested in perusing the Golang code that defines these objects can find them in [`machine_types.go`][link-5] (for the CAPI Machine object) and in [`awsmachine_types.go`][link-6] (for the CAPA AWSMachine object). For the KubeadmConfig object, the `kubeadm` bootstrap provider has its own repository [here][link-7], and the struct is defined in [`kubeadmbootstrapconfig_types.go`][link-8].

Finally, CAPI v1alpha2 still supports the MachineDeployment object (like a Deployment object is for Pods, but for Machines), but the underlying provider-specific objects are a bit different. Here's the example from the Quick Start:

```yaml
apiVersion: cluster.x-k8s.io/v1alpha2
kind: MachineDeployment
metadata:
  name: capi-quickstart-worker
  labels:
    cluster.x-k8s.io/cluster-name: capi-quickstart
    # Labels beyond this point are for example purposes,
    # feel free to add more or change with something more meaningful.
    # Sync these values with spec.selector.matchLabels and spec.template.metadata.labels.
    nodepool: nodepool-0
spec:
  replicas: 1
  selector:
    matchLabels:
      cluster.x-k8s.io/cluster-name: capi-quickstart
      nodepool: nodepool-0
  template:
    metadata:
      labels:
        cluster.x-k8s.io/cluster-name: capi-quickstart
        nodepool: nodepool-0
    spec:
      version: v1.15.3
      bootstrap:
        configRef:
          name: capi-quickstart-worker
          apiVersion: bootstrap.cluster.x-k8s.io/v1alpha2
          kind: KubeadmConfigTemplate
      infrastructureRef:
        name: capi-quickstart-worker
        apiVersion: infrastructure.cluster.x-k8s.io/v1alpha2
        kind: AWSMachineTemplate
---
apiVersion: infrastructure.cluster.x-k8s.io/v1alpha2
kind: AWSMachineTemplate
metadata:
  name: capi-quickstart-worker
spec:
  template:
    spec:
      instanceType: t3.large
      iamInstanceProfile: "nodes.cluster-api-provider-aws.sigs.k8s.io"
      sshKeyName: default
---
apiVersion: bootstrap.cluster.x-k8s.io/v1alpha2
kind: KubeadmConfigTemplate
metadata:
  name: capi-quickstart-worker
spec:
  template:
    spec:
      joinConfiguration:
        nodeRegistration:
          name: '{{ ds.meta_data.hostname }}'
          kubeletExtraArgs:
            cloud-provider: aws
```

What's new here that wasn't shown in previous examples are the AWSMachineTemplate and KubeadmConfigTemplate objects, which---as you might suspect---serve as templates for AWSMachine and KubeadmConfig objects for the individual Machines in the MachineDeployment. These template structures are defined in [`machinedeployment_types.go`][link-9] (for the CAPI MachineDeployment object), in [`kubeadmconfigtemplate_types.go`][link-10] (for the KubeadmConfigTemplate object), and in [`awsmachinetemplate_types.go`][link-11] (for the CAPA AWSMachineTemplate object).

I hope this information is useful. There's definitely more CAPI v1alpha2 content planned, and in the meantime feel free to browse [all CAPI-tagged articles][link-13]. If you have questions, comments, or corrections---we're all human and make mistakes from time to time---feel free to [contact me on Twitter][link-12]. Thanks!

[link-1]: https://kubernetes.io/
[link-2]: https://cluster-api.sigs.k8s.io/user/quick-start.html
[link-3]: https://github.com/kubernetes-sigs/cluster-api/blob/v0.2.1/api/v1alpha2/cluster_types.go
[link-4]: https://github.com/kubernetes-sigs/cluster-api-provider-aws/blob/v0.4.0/api/v1alpha2/awscluster_types.go
[link-5]: https://github.com/kubernetes-sigs/cluster-api/blob/v0.2.1/api/v1alpha2/machine_types.go
[link-6]: https://github.com/kubernetes-sigs/cluster-api-provider-aws/blob/v0.4.0/api/v1alpha2/awsmachine_types.go
[link-7]: https://github.com/kubernetes-sigs/cluster-api-bootstrap-provider-kubeadm/
[link-8]: https://github.com/kubernetes-sigs/cluster-api-bootstrap-provider-kubeadm/blob/v0.1.0/api/v1alpha2/kubeadmbootstrapconfig_types.go
[link-9]: https://github.com/kubernetes-sigs/cluster-api/blob/v0.2.1/api/v1alpha2/machinedeployment_types.go
[link-10]: https://github.com/kubernetes-sigs/cluster-api-bootstrap-provider-kubeadm/blob/v0.1.0/api/v1alpha2/kubeadmconfigtemplate_types.go
[link-11]: https://github.com/kubernetes-sigs/cluster-api-provider-aws/blob/v0.4.0/api/v1alpha2/awsmachinetemplate_types.go
[link-12]: https://twitter.com/scott_lowe
[link-13]: /tags/capi/
[xref-1]: {{< relref "2019-09-05-highly-available-kubernetes-clusters-on-aws-with-cluster-api.md" >}}
[xref-2]: {{< relref "2019-09-09-consuming-preexisting-aws-infrastructure-with-cluster-api.md" >}}
[xref-3]: {{< relref "2019-08-14-setting-up-aws-integrated-kubernetes-115-cluster-kubeadm.md" >}}
