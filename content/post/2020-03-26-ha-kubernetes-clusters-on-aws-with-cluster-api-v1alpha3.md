---
author: slowe
categories: Explanation
comments: true
date: 2020-03-26T09:00:00-07:00
tags:
- Kubernetes
- AWS
- CAPI
title: HA Kubernetes Clusters on AWS with Cluster API v1alpha3
url: /2020/03/26/ha-kubernetes-clusters-on-aws-with-cluster-api-v1alpha3/
---

A few weeks ago, I published a post on [HA Kubernetes clusters on AWS with Cluster API v1alpha2][xref-1]. That post was itself a follow-up to a post I wrote in September 2019 on [setting up HA clusters using Cluster API v1alpha1][xref-2]. In this post, I'll follow up on both of those posts with a look at setting up HA Kubernetes clusters on AWS using [Cluster API][link-1] v1alpha3. Although this post is similar to the v1alpha2 post, be aware there are some notable changes in v1alpha3, particularly with regard to the control plane.<!--more-->

If you're not yet familiar with Cluster API, take a look at [this high-level overview][xref-3] I wrote in August 2019. That post will provide an explanation of the project's goals as well as provide some terminology.

In this post, I won't discuss the process of establishing a management cluster; I'm assuming your Cluster API management cluster is already up and running. (I do have some articles in the content pipeline that discuss creating a management cluster.) Instead, this post will focus on creating a highly-available workload cluster. By "highly available," I mean a cluster with multiple control plane nodes that are distributed across multiple availability zones (AZs). Please read the "Disclaimer" section at the bottom for some caveats with regard to availability.

## Prerequisites

As mentioned above, this post assumes you already have a functional Cluster API management cluster. This post also assumes you have already installed the `kubectl` binary, and that you've configured `kubectl` to access your management cluster.

As a side note, I _highly_ recommend some sort of solution that shows you the current Kubernetes context in your shell prompt. I prefer [`powerline-go`][link-4], but choose/use whatever works best for you.

## Crafting Manifests for High Availability

The Cluster API manifests require two basic changes in order to be able to deploy a highly-available cluster:

1. The AWSCluster object needs to be modified to include information on how to create subnets across multiple AZs.
2. The manifests for worker nodes need to be modified to include AZ information.

You'll note that I didn't mention anything about the control plane above---that's because v1alpha3 introduces a new mechanism for managing the control plane of workload clusters. This new mechanism, the KubeadmControlPlane object, is smart enough in this release to automatically distribute control plane nodes across multiple AZs when they are present. (Nice, right?)

The next two sections will provide more details on the changes listed above.

### Creating Subnets Across Multiple AZs

By default, Cluster API will only create public and private subnets in the first AZ it finds in a region. As a result, all control plane nodes and worker nodes will end up in the same AZ. To change that, you'll need to modify the AWSCluster object to tell Cluster API to create subnets across multiple AZs.

This change is accomplished by adding a `networkSpec` to the AWSCluster specification. Here's an example `networkSpec`:

```yaml
spec:
  networkSpec:
    vpc:
      cidrBlock: 10.10.0.0/16
    subnets:
    - availabilityZone: us-west-2a
      cidrBlock: 10.10.0.0/20
      isPublic: true
    - availabilityZone: us-west-2a
      cidrBlock: 10.10.16.0/20
    - availabilityZone: us-west-2b
      cidrBlock: 10.10.32.0/20
      isPublic: true
    - availabilityZone: us-west-2b
      cidrBlock: 10.10.48.0/20
    - availabilityZone: us-west-2c
      cidrBlock: 10.10.64.0/20
      isPublic: true
    - availabilityZone: us-west-2c
      cidrBlock: 10.10.80.0/20
```

This YAML is fairly straightforward, and is largely (completely?) unchanged from previous versions of Cluster API. One key takeaway is that the user is responsible for "manually" breaking down the VPC CIDR appropriately for the subnets in each AZ.

With this change, Cluster API will now create multiple subnets within a VPC, distributing those subnets across AZs as directed. Since multiple AZs are now accessible by Cluster API, the control plane nodes (managed by the KubeadmControlPlane object) will automatically get distributed across AZs. This leaves only the worker nodes to distribute, which I'll discuss in the next section.

### Distributing Worker Nodes Across AZs

To distribute worker nodes across AZs, users can add a `failureDomain` field to their Machine or MachineDeployment manifests. This field specifies the name of an AZ where a usable subnet exists. Using the example AWSCluster specification listed above, this means I could tell Cluster API to use us-west-2a, us-west-2b, or us-west-2c. I could _not_ tell Cluster API to use us-west-2d, because there are no Cluster API-usable subnets in that AZ.

For a Machine object, the `failureDomain` field goes in the Machine's specification:

```yaml
spec:
  failureDomain: "us-west-2a"
```

For a MachineDeployment, the `failureDomain` field goes in the template specification:

```yaml
spec:
  template:
    spec:
      failureDomain: "us-west-2b"
```

Given the nature of a MachineDeployment, it's not possible to distribute Machines from a single MachineDeployment across AZs. To use MachineDeployments with multiple AZs, you'd need to use a separate MachineDeployment for each AZ.

With these two changes---adding a `networkSpec` to the AWSCluster object and adding a `failureDomain` field to your Machine or MachineDeployment objects---Cluster API will instantiate a Kubernetes cluster whose control plane nodes and worker nodes are distributed across multiple AWS AZs.

## Disclaimer

Readers should note that deploying across multiple AZs is not a panacea to cure all availability ills. Although the loss of a single AZ will not (generally) render the cluster unavailable---etcd will maintain a quorum so the API server will continue to function---the control plane may be flooded with the demands of rescheduling Pods, and remaining active nodes may not be able to support the resource requirements of the Pods being rescheduled. The sizing and overall utilization of the cluster will greatly affect the behavior of the cluster and the workloads hosted there in the event of an AZ failure. Careful planning is needed to maximize the availability of the cluster even in the face of an AZ failure. There are also other considerations, like cross-AZ traffic charges, that should be taken into account. There is no "one size fits all" solution.

I hope this post is helpful. If you have questions, please do reach out to me on [the Kubernetes Slack instance][link-2] (I hang out a lot in the `#kubeadm` and `#cluster-api-aws` channels), or reach out to [me on Twitter][link-3]. I'd love to hear from you, and possibly help you if I can.

[link-1]: https://cluster-api.sigs.k8s.io/
[link-2]: https://kubernetes.slack.com/
[link-3]: https://twitter.com/scott_lowe
[link-4]: https://github.com/justjanne/powerline-go
[xref-1]: {{< relref "2020-03-05-ha-kubernetes-clusters-on-aws-with-cluster-api-v1alpha2.md" >}}
[xref-2]: {{< relref "2019-09-05-highly-available-kubernetes-clusters-on-aws-with-cluster-api.md" >}}
[xref-3]: {{< relref "2019-08-26-an-introduction-to-kubernetes-cluster-api.md" >}}
