---
author: slowe
categories: Explanation
comments: true
date: 2019-09-09T12:00:00Z
tags:
- Kubernetes
- AWS
- CAPI
- Terraform
- Pulumi
title: Consuming Pre-Existing AWS Infrastructure with Cluster API
url: /2019/09/09/consuming-preexisting-aws-infrastructure-with-cluster-api/
---

All the posts I've published so far about [Kubernetes][link-2] [Cluster API (CAPI)][link-1] assume that the underlying infrastructure needs to be created. This is fine, because generally speaking that's part of the value of CAPI---it will create new cloud infrastructure for every Kubernetes cluster it instantiates. In the case of AWS, this includes VPCs, subnets, route tables, Internet gateways, NAT gateways, Elastic IPs, security groups, load balancers, and (of course) EC2 instances. But what if you _didn't_ want CAPA to create AWS infrastructure? In this post, I'll show you how to consume pre-existing AWS infrastructure with Cluster API for AWS (CAPA).<!--more-->

Why would one _not_ want CAPA to create the necessary AWS infrastructure? There are a variety of reasons, but the one that jumps to my mind immediately is that an organization may have established/proven expertise and a process around the use of infrastructure-as-code (IaC) tooling like [Terraform][link-3], [CloudFormation][link-4], or [Pulumi][link-5]. In cases like this, such organizations would very likely prefer to continue to use the tooling they already know and with which they are already familiar, instead of relying on CAPA. Further, the use of third-party IaC tooling may allow for greater customization of the infrastructure than CAPA would allow.

Fortunately, CAPA makes it reasonably straightforward to use pre-existing infrastructure. The key here is the `networkSpec` object in a Cluster definition. Readers who read the post on [highly available clusters on AWS with Cluster API][xref-1] saw how to use the `networkSpec` object to tell CAPA how to create subnets across multiple availability zones (AZs) for greater availability. The `networkSpec` object can _also_ be used to tell CAPA how to use pre-existing infrastructure.

For this second use case (having CAPA use pre-existing infrastructure), users will need to add a `networkSpec` object to the Cluster definition that provides the IDs of the VPC and the subnets within the VPC that CAPA should use. Here's an example (this example assumes CAPI v1alpha1; the full "path" to where `networkSpec` should be specified changes in CAPI v1alpha2):

```yaml
spec:
  providerSpec:
    value:
      networkSpec:
        vpc:
          id: vpc-0425c335226437144
        subnets:
          - id: subnet-07758a9bc904d06af
          - id: subnet-0a3507a5ad2c5c8c3
          - id: subnet-02ad6429dd0532452
          - id: subnet-02b300779e9d895cf
          - id: subnet-03d8f353b289b025f
          - id: subnet-0a2fe03b0d88fa078
```

This example provides CAPA with the ID of a VPC to use, as well as the IDs for public and private subnets across three different AZs. Note that these need to be fully functional subnets, so users need to be sure to have created not only the VPC and the subnets but also the necessary Internet gateways (for public subnets), NAT gateways (for private subnets), Elastic IP addresses (for the NAT gateways), route tables, and route table associations. Cluster API will take care of security groups, load balancers, and EC2 instances; these do not need to be created in advance.

In addition to ensuring that the infrastructure is fully functional, users must be sure that the appropriate AWS tags are added to all objects. When CAPI creates an AWS infrastructure object, it adds the `sigs.k8s.io/cluster-api-provider-aws/cluster/<cluster-name>` tag, with a value of "managed", and the `sigs.k8s.io/cluster-api-provider-aws/role` tag, with a value of "common." Users will want to ensure whatever tooling they are using to create the infrastructure to be consumed by CAPA has those tags, although the values may be different (it's not required to use "managed" and "common", respectively).

With the infrastructure created and tagged, and the `networkSpec` information in place, applying the Cluster YAML definition against the management cluster using `kubectl` will generate a new Kubernetes cluster that will leverage the specified VPC and subnets instead of creating a new VPC and subnets. (Users could also use a configuration like this with `clusterctl` when creating a new management cluster.)

Users who are configuring CAPA to consume pre-existing infrastructure still have the option of instructing CAPA to distribute EC2 instances across multiple availability zones (AZs) for greater availability (as described [here][xref-1]). In this case, users have two options for providing CAPA with the necessary information to distribute the instances across multiple AZs:

1. By specifying an AZ in the Machine specification
2. By specifying a subnet ID in the Machine specification

The first option is exactly as described in [the previous post on HA clusters][xref-1], so I won't repeat it here.

The second option is only available in the case of having CAPA consume pre-existing infrastructure, because only in such cases will the subnet IDs be known in advance. To add the subnet ID to the Machine YAML specification would look something like this (this example is for CAPI v1apha1; the full "path" in the YAML manifest changes for CAPI v1alpha2):

```yaml
spec:
  providerSpec:
    value:
      subnet:
        id: subnet-0a3507a5ad2c5c8c3
```

This is a bit more tedious than specifying AZ; here, the user must manually look up the subnet IDs and determine in which AZ each subnet resides. Further, since both public and private subnets are needed, users must manually determine which subnet IDs belong to public subnets and which belong to private subnets.(As noted in [this post][xref-1], placing instances across multiple AZs is not the answer for all availability needs---see the "Disclaimer" portion of the post.)

Finally, users will need to provide their "own" bastion host for accessing the EC2 instances that CAPA places on private subnets. When configuring CAPA to use pre-existing infrastructure, CAPA will _not_ create a bastion host automatically.

If anyone has any feedback, corrections, or suggestions for improving this post, please feel free to [contact me via Twitter][link-99]. All feedback is welcome!

**UPDATE 13 September 2019:** I've updated this post to point out that the examples provided are based on CAPI v1alpha1. The examples will work with CAPI v1alpha2, but the "path" within the YAML manifest changes with CAPI v1alpha2.

**UPDATE 22 April 2020:** The [upstream documentation for using existing AWS infrastructure][link-6] has been updated and should be considered the authoritative source of information on this topic.

[link-1]: https://github.com/kubernetes-sigs/cluster-api
[link-2]: https://kubernetes.io/
[link-3]: https://www.terraform.io/
[link-4]: https://aws.amazon.com/cloudformation/
[link-5]: https://www.pulumi.com/
[link-6]: https://github.com/kubernetes-sigs/cluster-api-provider-aws/blob/master/docs/existing-aws-infrastructure.md
[link-99]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2019-09-05-highly-available-kubernetes-clusters-on-aws-with-cluster-api.md" >}}
