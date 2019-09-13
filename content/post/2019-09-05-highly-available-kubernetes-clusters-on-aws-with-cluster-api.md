---
author: slowe
categories: Explanation
comments: true
date: 2019-09-05T11:00:00Z
tags:
- Kubernetes
- AWS
- CAPI
title: Highly Available Kubernetes Clusters on AWS with Cluster API
url: /2019/09/05/highly-available-kubernetes-clusters-on-aws-with-cluster-api/
---

In [my previous post][xref-1] on [Kubernetes][link-2] Cluster API, I showed readers how to use the [Cluster API provider for AWS][link-4] (referred to as CAPA) to instantiate a Kubernetes cluster on AWS. Readers who followed through the instructions in that post may note CAPA places all the nodes for a given cluster in a single AWS availability zone (AZ) by default. While multi-AZ Kubernetes deployments are not without their own considerations, it's generally considered beneficial to deploy across multiple AZs for higher availability. In this post, I'll share how to deploy highly-available Kubernetes clusters---defined as having multiple control plane nodes distributed across multiple AZs---using Cluster API for AWS (CAPA).<!--more-->

This post assumes that you have already deployed a management cluster, so the examples may mention using `kubectl` to apply CAPA manifests against the management cluster to deploy a highly-available workload cluster. However, the information needed in the CAPA manifests would also work with `clusterctl` in order to deploy a highly-available management cluster, although users should keep in mind that `clusterctl` is deprecated with the CAPI v1alpha2 release. (Not familiar with what I mean when I say "management cluster" or "workload cluster"? Be sure to go read [the introduction to Cluster API post][xref-2] first.)

Also, this post was written with CAPI v1alpha1 in mind. Although the CAPI v1alpha2 release does change quite a bit with regard to the YAML manifests and the fields contained therein, it appears that the specific sections/fields needed to deploy highly-available clusters remains the same between v1alpha1 and v1alpha2.

Two changes are needed to a set of CAPA manifests in order to deploy a highly-available cluster:

1. The YAML manifest for the Cluster object has to provide the specification for how to arrange the subnets and IP addresses across AZs.
2. The YAML manifest for the control plane nodes has to specify the AZ for each control plane node (thus allowing the user/operator to distribute them across AZs).

The first of these changes is modifying the YAML manifest for the Cluster object. By default, the Cluster YAML manifest doesn't provide any information on how to assign subnets or IP addresses across multiple AZs, so CAPA does everything within the first AZ of a region. In order to make CAPA deploy across multiple AZs, users must extend the Cluster YAML definition to include that information via a `networkSpec` (a Network specification). Since the  `generate-yaml.sh` script included with v1alpha1 of CAPA doesn't do this, the addition of network information has to be done by the user/operator.

Here's an example YAML snippet that would lay out subnets across three AZs in the AWS "us-west-2" region (this example assumes v1alpha1; the full "path" to where `networkSpec` should be specified changes in v1alpha2):

```yaml
spec:
  value:
    providerSpec:
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

So far, so good! This is only half the picture, though---users also need to tell CAPA how to deploy nodes across the AZs. That's the second required change: adding AZ information to the Machine specification for the control plane nodes. Additionally, users should make sure they are defining multiple control plane nodes using a MachineList specification, instead of defining only a single control plane node with a simple Machine specification. The `generate-yaml.sh` script supplied with the CAPA example manifests generates an example MachineList for the control plane as `controlplane-machines-ha.yaml` users can use as a starting point/basis for an HA control plane manifest.

To specify an AZ for instances created by CAPA, users would add this snippet of YAML in a Machine definition (this example assumes v1alpha1; the full "path" to where users specify this changes in v1alpha2):

```yaml
spec:
  providerSpec:
    value:
      availabilityZone: "us-west-2a"
```

When this snippet of YAML is applied to a management cluster as part of a Machine or MachineList specification, CAPA will look up the subnet(s) associated with that AZ and instantiate the instance in the appropriate subnet. Presto! Users now have Kubernetes control plane that contains multiple nodes distributed across multiple AZs.

This same change (adding AZ information) can be used to also distribute worker nodes across multiple AZs. This would not work with a MachineDeployment (as all the Machines created by a MachineDeployment are identical), but users could use separate MachineDeployments for each AZ, or use a MachineList and provide the AZ information for each Machine in the MachineList specification.

## Disclaimer

Readers need to be aware that deploying across multiple AZs is not a panacea to cure all availability ills. Although the loss of a single AZ will not (generally) render the cluster unavailable---etcd will maintain a quorum so the API server will continue to function---the control plane may be flooded with the demands of rescheduling Pods, and remaining active nodes may not be able to support the resource requirements of the Pods being rescheduled. The sizing and overall utilization of the cluster will greatly affect the behavior of the cluster and the workloads hosted there in the event of an AZ failure. Careful planning is needed to maximize the availability of the cluster even in the face of an AZ failure.

Have questions? Spotted an error in this post? Feel free to [contact me via Twitter][link-3]; all feedback is welcome.

**UPDATE 13 September 2019:** I've updated this post with some information from the CAPI v1alpha2 release.

[link-1]: /tags/capi/
[link-2]: https://kubernetes.io/
[link-3]: https://twitter.com/scott_lowe
[link-4]: https://github.com/kubernetes-sigs/cluster-api-provider-aws
[xref-1]: {{< relref "2019-08-27-bootstrapping-a-kubernetes-cluster-on-aws-with-clusterapi.md" >}}
[xref-2]: {{< relref "2019-08-26-an-introduction-to-kubernetes-cluster-api.md" >}}
