---
author: slowe
categories: Explanation
comments: true
date: 2020-04-22T16:00:00-07:00
tags:
- AWS
- CAPI
- Security
- Kubernetes
title: Using Existing AWS Security Groups with Cluster API
url: /2020/04/22/using-existing-aws-security-groups-with-cluster-api/
---

I've written before about [how to use existing AWS infrastructure with Cluster API (CAPI)][xref-1], and I was recently able to help update [the upstream documentation][link-3] on this topic (the upstream documentation should now be considered the authoritative source). These instructions are perfect for placing a Kubernetes cluster into an existing VPC and associated subnets, but there's one scenario that they don't yet address: what if you need your CAPI workload cluster to be able to communicate with other EC2 instances or other AWS services in the same VPC? In this post, I'll show you the CAPI functionality that makes this possible.<!--more-->

One of the primary mechanisms used in AWS to control communications among instances and services is the _security group._ I won't go into any detail on security groups, but [this page from AWS][link-4] provides an explanation and overview of how security groups work.

In order to make a CAPI workload cluster able to communicate with other EC2 instances or other AWS services, you'll need to somehow use security groups to make that happen. There are at least two---possibly more---ways to accomplish this:

1. _You could add other instances or services to the CAPI-created security groups._ The Cluster API Provider for AWS (CAPA) automatically creates a set of security groups for use by the EC2 instances and ELBs created when it bootstraps a workload cluster. You could manually add other instances or services into these groups to enable connectivity, but this approach is fraught with problems (not the least of which is coupling two things with different lifecycles).
2. _You could add CAPI-created instances to pre-existing security groups._ This solves the problem of splitting the lifecycle of the workload cluster from the connectivity needs of workloads outside the cluster, but how do you have this happen automatically when new workload clusters are provisioned? Any sort of manual operations just won't scale, and will become a potential source of failures and outages down the road.

Fortunately for us and for this blog post, CAPI provides an answer to how to take approach #2 above and automate it. In reviewing [the API specification for CAPA v1alpha3][link-1], I noted that the AWSMachine spec includes a way to reference additional AWS security groups. 

For example, in an AWSMachine spec, you could do the following:

```yaml
spec:
  additionalSecurityGroups:
    - id: <existing_security_group_id>
```

This will result in an EC2 instance that _joins the specified existing security group_ in addition to the CAPI-managed security groups. This provides exactly the behavior you're seeking---the ability to have CAPI-managed resources automatically join an existing security group to enable access to other EC2 instances or AWS services.

Since the AWSMachine spec is also used by an AWSMachineTemplate, and since AWSMachineTemplates are used by both MachineDeployments (to manage groups of worker nodes) and [the new v1alpha3 KubeadmControlPlane][link-2] (to manage multiple control plane nodes as a single entity), this means we can leverage the same functionality for AWSMachineTemplates:

```yaml
spec:
  template:
    spec:
      additionalSecurityGroups:
        - id: <existing_security_group_id>
```

Adding the snippet of YAML above to an AWSMachineTemplate definition would end up with multiple EC2 instances---managed by either a MachineDeployment or the KubeadmControlPlane---that automatically join the specified, pre-existing security group. To specify multiple security groups, you just add more entries under `additionalSecurityGroups`.

Therefore, if you have an existing VPC (and all associated subnets, gateways, routes, etc.) that you want to use with CAPI, _and_ there are existing EC2 instances and other workloads in that VPC that need to communicate with the CAPI workload cluster, using this `additionalSecurityGroups` field in your AWSMachine and AWSMachineTemplate definitions can accomplish exactly what you need.

I hope this information is useful. If you have questions, I would love to try to help. Find [me on Twitter][link-6], or hit me up on [the Kubernetes Slack instance][link-5]. I'm also open to hear any feedback or suggestions for improvement. Thanks!

[link-1]: https://godoc.org/github.com/kubernetes-sigs/cluster-api-provider-aws/api/v1alpha3
[link-2]: https://godoc.org/sigs.k8s.io/cluster-api/controlplane/kubeadm/api/v1alpha3
[link-3]: https://github.com/kubernetes-sigs/cluster-api-provider-aws/blob/master/docs/existing-aws-infrastructure.md
[link-4]: https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html
[link-5]: https://kubernetes.slack.com
[link-6]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2019-09-09-consuming-preexisting-aws-infrastructure-with-cluster-api.md" >}}
