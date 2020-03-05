---
author: slowe
categories: Explanation
comments: true
date: 2020-03-05T12:30:00-07:00
tags:
- Kubernetes
- AWS
- CAPI
title: HA Kubernetes Clusters on AWS with Cluster API v1alpha2
url: /2020/03/05/ha-kubernetes-clusters-on-aws-with-cluster-api-v1alpha2/
---

About six months ago, I wrote [a post][xref-2] on how to use [Cluster API][link-1] (specifically, the [Cluster API Provider for AWS][link-4]) to establish highly available [Kubernetes][link-2] clusters on AWS. That post was written with Cluster API (CAPI) v1alpha1 in mind. Although the concepts I presented there worked with v1alpha2 (released shortly after that post was written), I thought it might be helpful to revisit the topic with CAPI v1alpha2 specifically in mind. So, with that, here's how to establish highly available Kubernetes clusters on AWS using CAPI v1alpha2.<!--more-->

By the way, it's worth pointing out that CAPI is a fast-moving project, and release candidate versions of CAPI v1alpha3 are already available for some providers. Keep that in mind if you decide to start working with CAPI. I will write an updated version of this post for v1alpha3 once it has been out for a little while.

To be sure we're all speaking the same language, here are some terms/acronyms that I'll use in this post:

* CAPI = Cluster API (the provider-independent parts)
* CAPA = Cluster API Provider for AWS
* CABPK = Cluster API Bootstrap Provider for Kubeadm (new for v1alpha2)
* Management cluster = a Kubernetes cluster that has the CAPI, CAPA, and CABPK components installed and is prepared to manage workload clusters
* Workload cluster = a Kubernetes cluster whose lifecycle is managed by a management cluster

If you're not familiar at all with CAPI, I recommend starting [here][xref-1]. After you've read that, proceed with the rest of this article.

## Prerequisites

This post assumes that you have already deployed a management cluster, so the examples may mention using `kubectl` to apply CAPA manifests against the management cluster to deploy a highly-available workload cluster. This post also assumes you have access to the `kubectl` binary, either locally (easiest) or on the management cluster (also acceptable). For the purposes of this post, I'll assume readers are using the `kubectl` binary locally.

## Modifying the Cluster API Manifests

Two changes are needed to a set of CAPA manifests in order to deploy a highly-available cluster:

1. The YAML manifest for the AWSCluster object has to provide the specification for how to arrange the subnets and IP addresses across AZs.
2. The YAML manifest for the control plane nodes has to specify the AZ for each control plane node (thus allowing the user/operator to distribute them across AZs).

Let's take a look at each of these changes in a bit more detail.

### Modifying the AWSCluster Object

The first of these changes is modifying the YAML manifest for the AWSCluster object. (Note this is a change from v1alpha1, where users needed to modify the Cluster object.) By default, the Cluster YAML manifest doesn't provide any information on how to assign subnets or IP addresses across multiple AZs, so CAPA does everything within the first AZ of a region. In order to make CAPA deploy across multiple AZs, users must extend the AWSCluster YAML definition to include that information via a `networkSpec` (a Network specification).

Here's an example YAML snippet for an AWSCluster object that would lay out subnets across three AZs in the AWS "us-west-2" region:

```yaml
spec:
  networkSpec:
    vpc:
      cidrBlock: "10.20.0.0/16"
    subnets:
    - availabilityZone: us-west-2a
      cidrBlock: "10.20.0.0/20"
      isPublic: true
    - availabilityZone: us-west-2a
      cidrBlock: "10.20.16.0/20"
    - availabilityZone: us-west-2b
      cidrBlock: "10.20.32.0/20"
      isPublic: true
    - availabilityZone: us-west-2b
      cidrBlock: "10.20.48.0/20"
    - availabilityZone: us-west-2c
      cidrBlock: "10.20.64.0/20"
      isPublic: true
    - availabilityZone: us-west-2c
      cidrBlock: "10.20.80.0/20"
```

This section of YAML provides the overall CIDR block for the VPC that CAPA creates, as well as specifying the CIDR blocks for each subnet in each AZ. This does mean that users will have to "manually" break down the CIDR across the AZs and subnets. This YAML shows both public and private subnets in each AZ, which are required in order for CAPA to create a NAT gateway (for private instances) in each AZ.

When this YAML is applied to the management cluster using `kubectl`, CAPA will create the VPC, subnets, routes and route tables, and Internet or NAT gateways (for public or private subnets, respectively).

### Modifying the Control Plane AWSMachine Objects

So far, so good! This is only half the picture, though---even though CAPA has created infrastructure across multiple AZs, users also need to tell CAPA how to deploy nodes across the AZs. That's the second required change: adding AZ information to the AWSMachine specification for the control plane nodes. Additionally (and somewhat obviously), users should make sure they are defining multiple control plane nodes, instead of defining only a single control plane node.

To specify an AZ for instances created by CAPA, users would add this snippet of YAML in an AWSMachine definition:

```yaml
spec:
  availabilityZone: "us-west-2a"
```

When this snippet of YAML is added to an AWSMachine specification and applied to a management cluster, CAPA will look up the subnet(s) associated with that AZ and instantiate the instance in the appropriate subnet. Note that CAPA will select private subnets over public subnets in cases like this.

It's important to note that changes in the upcoming v1alpha3 release will mean that this approach of distributing control plane nodes across AZs will **not** work. Stay tuned for more details once v1alpha3 is available and I've had some time to work with it.

### Modifying Worker Node Objects

This same change (adding AZ information) can be used to also distribute worker nodes across multiple AZs. If you are specifying your worker nodes individually as Machine and AWSMachine objects, then the change in the previous section (adding an `availabilityZone` parameter to the `spec` for the AWSMachine object) will work for worker nodes as well.

Note that the nature of a MachineDeployment means that all Machines in a MachineDeployment will reside in the same AZ (because all Machines are spawned from the same template), but users could use separate MachineDeployments for each AZ. In that case, the snippet of YAML to add looks like this:

```yaml
spec:
  template:
    spec:
      availabilityZone: "us-west-2b"
```

You would add this snippet to the AWSMachineTemplate for the MachineDeployment. To ensure you had worker nodes across multiple AZs, you'd then need a separate MachineDeployment (and corresponding AWSMachineTemplate) for each AZ, as mentioned earlier.

## Disclaimer

Readers need to be aware that deploying across multiple AZs is not a panacea to cure all availability ills. Although the loss of a single AZ will not (generally) render the cluster unavailable---etcd will maintain a quorum so the API server will continue to function---the control plane may be flooded with the demands of rescheduling Pods, and remaining active nodes may not be able to support the resource requirements of the Pods being rescheduled. The sizing and overall utilization of the cluster will greatly affect the behavior of the cluster and the workloads hosted there in the event of an AZ failure. Careful planning is needed to maximize the availability of the cluster even in the face of an AZ failure. There are also other considerations, like cross-AZ traffic charges, that should be taken into account.

Have questions? Spotted an error in this post? Feel free to [contact me via Twitter][link-3]; I welcome all constructive feedback.

[link-1]: https://github.com/kubernetes-sigs/cluster-api/
[link-2]: https://kubernetes.io/
[link-3]: https://twitter.com/scott_lowe
[link-4]: https://github.com/kubernetes-sigs/cluster-api-provider-aws
[xref-1]: {{< relref "2019-08-26-an-introduction-to-kubernetes-cluster-api.md" >}}
[xref-2]: {{< relref "2019-09-05-highly-available-kubernetes-clusters-on-aws-with-cluster-api.md" >}}
