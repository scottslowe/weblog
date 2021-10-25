---
author: slowe
categories: Tutorial
comments: true
date: 2021-10-25T09:45:00-06:00
tags:
- AWS
- CAPI
- Kubernetes
title: Influencing Cluster API AMI Selection
url: /2021/10/25/influencing-cluster-api-ami-selection/
---

The [Kubernetes Cluster API (CAPI) project][link-1]---which recently released v1.0---can, if you wish, help manage the underlying infrastructure associated with a cluster. (You're also fully able to have CAPI [use existing infrastructure][link-4] as well.) Speaking specifically of [AWS][link-2], this means that the [Cluster API Provider for AWS][link-3] is able to manage VPCs, subnets, routes and route tables, gateways, and---of course---EC2 instances. These EC2 instances are booted from a set of AMIs (Amazon Machine Images, definitely pronounced "ay-em-eye" with [three syllables][link-5]) that are prepared and maintained by the CAPI project. In this short and simple post, I'll show you how to influence the AMI selection process that CAPI's AWS provider uses.<!--more-->

There are a couple different ways to influence AMI selection, and all of them have to do with settings within [the AWSMachineSpec][link-6], which controls the configuration of an AWSMachine object. (An AWSMachine object is an infrastructure-specific implementation of a logical Machine object.) Specifically, there are these options for influencing AMI selection:

1. You can instruct CAPI to use a specific AMI with the `ami` field. (If this field is set, the other options do not apply.)
2. You can modify the lookup format used to find an AMI with the `imageLookupFormat` field. By default, the value for this field is `capa-ami-{{.BaseOS}}-?{{.K8sVersion}}-*`. These placeholders are controlled by the `imageLookupBaseOS` field (described below) and the Kubernetes version supplied by a Machine, MachineDeployment, or KubeadmControlPlane object.
3. The `imageLookupOrg` field allows you to provide an AWS Organization ID to use in looking up an AMI. If I'm not mistaken, this value defaults to "258751437250".
4. You can provide parameters for the base OS lookup using the `imageLookupBaseOS` field. For example, to have the CAPI AWS provider use Ubuntu 20.04 instead of Ubuntu 18.04 (the default), add this field with a value of "ubuntu-20.04" to your CAPI manifests.

(For reference, the default values for `imageLookupOrg`, `imageLookupBaseOS`, and `imageLookupFormat` are found [here][link-7] in the code.)

Using these fields, you could instruct CAPI to use a custom Ubuntu 20.04-based AMI maintained by your own AWS account by specifying `imageLookupOrg`, `imageLookupBaseOS`, and `imageLookupFormat`. I personally have used the `imageLookupBaseOS` field to use Ubuntu 20.04 instead of Ubuntu 18.04 on several occasions.

Keep in mind these fields are part of the AWSMachineSpec struct, and may be used in AWSMachine objects as well as AWSMachineTemplate objects.

If you have any questions---or if I have misrepresented something or explained it incorrectly---feel free to contact [me on Twitter][link-8] or find me on [the Kubernetes Slack community][link-9]. All constructive feedback is welcome---I'd love to hear from you.

[link-1]: https://cluster-api.sigs.k8s.io/
[link-2]: https://aws.amazon.com/
[link-3]: https://cluster-api-aws.sigs.k8s.io/
[link-4]: https://cluster-api-aws.sigs.k8s.io/topics/consuming-existing-aws-infrastructure.html
[link-5]: https://blog.technodrone.cloud/2019/03/ami-has-3-syllables-ami-aws.html
[link-6]: https://pkg.go.dev/sigs.k8s.io/cluster-api-provider-aws/api/v1beta1#AWSMachineSpec
[link-7]: https://github.com/kubernetes-sigs/cluster-api-provider-aws/blob/v1.0.0/pkg/cloud/services/ec2/ami.go#L38-L53
[link-8]: https://twitter.com/scott_lowe
[link-9]: https://kubernetes.slack.com
