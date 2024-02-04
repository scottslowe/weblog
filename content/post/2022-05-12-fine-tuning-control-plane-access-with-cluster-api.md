---
author: slowe
categories: Explanation
comments: true
date: 2022-05-12T14:30:00-06:00
tags:
- AWS
- CAPI
- Kubernetes
- Networking
- Security
title: Fine-Tuning Control Plane Access with Cluster API
url: /2022/05/12/fine-tuning-control-plane-access-with-cluster-api/
---

When [Cluster API][link-1] creates a workload cluster, it also creates a load balancing solution to handle traffic to the workload cluster's control plane. This is necessary so that the control plane endpoint is decoupled from the underlying control plane nodes (which facilitates scaling the control plane, among other things). On AWS, this mean creating an ELB and a set of security groups. For flexibility, Cluster API provides a limited ability to customize this control plane load balancer. In this post, I'll show you how to use this functionality to fine-tune access to a workload cluster's control plane when using Cluster API with AWS.<!--more-->

If you're not familiar with Cluster API (hereafter just referred to as "CAPI"), then [my introduction to CAPI article][xref-1] may be useful. Keep in mind that article was written in 2019, while the project was still in its early stages. The high-level concepts are correct, but some of the details may have shifted slightly over the last three years as the project progressed from `v1alpha1` APIs to the now-current `v1beta1` APIs.

The key here is the `controlPlaneLoadBalancer` object, which is part of the `AWSCluster` object (see details [here in the code][link-2] or [here via `pkg.go.dev`][link-3]). With regard to access to a workload cluster's control plane via its control plane load balancer, two fields are of particular interest:

1. The `additionalSecurityGroups` field allows you to list any additional security groups---referenced by security group ID---that the control plane load balancer should use (this is in addition to the standard set of security groups created by CAPI). This is an optional field.
2. The `scheme` field allows you to specify whether the load balancer should be externally-accessible from the Internet (specified with the value "internet-facing", which is the default value) or not (specified with the value "internal"). If you specify "internal" as the scheme, then the load balancer won't get a public IP address and won't be reachable via the Internet. This would be a great way to restrict access to only those connections coming via a VPN (or similar) into the VPC.

Personally, I would _at least_ switch the scheme on the control plane load balancer; otherwise, the API server of your newly-created workload cluster is publicly accessible via the Internet. You can then leverage any additional security groups as needed.

One note to keep in mind: the default security group created by the AWS provider for CAPI (referred to as CAPA) allows access from `0.0.0.0/0` to the control plane load balancer. If you need to _restrict_ access to something more fine-grained than that, using the `additionalSecurityGroups` field is not the way. As outlined [here][link-6], you'll instead want to use the `securityGroupOverrides` field (which is found at `spec.network.securityGroupOverrides`).

I hope this information is useful. Feel free to reach out to me if you have any questions (or corrections, in the event I explained/presented something incorrectly). You can find me on [the Kubernetes Slack][link-4] (as well as a number of other Slack communities) as well as [on Twitter][link-5].

[link-1]: https://cluster-api.sigs.k8s.io
[link-2]: https://github.com/kubernetes-sigs/cluster-api-provider-aws/blob/master/api/v1beta1/awscluster_types.go#L55-L57
[link-3]: https://pkg.go.dev/sigs.k8s.io/cluster-api-provider-aws/api/v1beta1#AWSClusterSpec
[link-4]: https://kubernetes.slack.com
[link-5]: https://twitter.com/scott_lowe
[link-6]: https://cluster-api-aws.sigs.k8s.io/topics/bring-your-own-aws-infrastructure.html#security-groups
[xref-1]: {{< relref "2019-08-26-an-introduction-to-kubernetes-cluster-api.md" >}}
