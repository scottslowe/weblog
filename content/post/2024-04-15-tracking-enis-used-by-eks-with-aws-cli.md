---
author: slowe
categories: Explanation
comments: true
date: 2024-04-15T12:30:00-06:00
tags:
- AWS
- CLI
- EKS
- Kubernetes
- Networking
title: Tracking ENIs used by EKS with AWS CLI
url: /2024/04/15/tracking-enis-used-by-eks-with-aws-cli/
---

I've recently been spinning up lots of Amazon Elastic Kubernetes Service (EKS) clusters (using Pulumi, of course) in order to test various Cilium configurations. Along the way, I've wanted to verify the association and configuration of Elastic Network Interfaces (ENIs) being used by the EKS cluster. In this post, I'll share a couple of AWS CLI commands that will help you track the ENIs used by an EKS cluster.<!--more-->

When I first set out to find the easiest way to track the ENIs used by the nodes in an [EKS][link-1] cluster, I thought that AWS resource tags might be the key. I was right---but not in the way I expected. In the [Pulumi][link-2] program (written in [Go][link-3]) that I use to create EKS clusters, I made sure to tag all the resources.

For example, when defining the EKS cluster itself I assigned tags:

```go
eksCluster, err := eks.NewCluster(ctx, "eks-cluster", &eks.ClusterArgs{
    Name:    pulumi.Sprintf("%s-test", regionNames[awsRegion]),
    // Some code omitted here for brevity
    Tags: pulumi.StringMap{
        "Name":   pulumi.Sprintf("%s-test", regionNames[awsRegion]),
        "owner":  pulumi.String(ownerTag),
        "team":   pulumi.String(teamTag),
        "usage":  pulumi.String(usageTag),
        "expiry": pulumi.String("2025-01-01"),
    },
})
```

And I assigned tags again when defining the node group for the EKS cluster:

```go
_, err = eks.NewNodeGroup(ctx, "node-group", &eks.NodeGroupArgs{
    ClusterName:   eksCluster.Name,
    // Some code omitted here for brevity
    Tags: pulumi.StringMap{
        "Name":   pulumi.Sprintf("%s-nodegroup-01", regionNames[awsRegion]),
        "owner":  pulumi.String(ownerTag),
        "team":   pulumi.String(teamTag),
        "usage":  pulumi.String(usageTag),
        "expiry": pulumi.String("2025-01-01"),
    },
})
```

I _thought_ that these tags would carry over to the ENIs attached to the EC2 instances in the node group. Assuming the value of `ownerTag` was set to "slowe", it would be possible to see all the ENIs with this command:

```bash
aws ec2 describe-network-interfaces --filters Name=tag:owner,Values=slowe
```

Alas, these tags don't carry over (not that I've observed, anyway). However, all is not lost! EKS creates its own tag you can use with the `describe-network-interfaces` command:

```bash
aws ec2 describe-network-interfaces \
--filters Name=tag:cluster.k8s.amazonaws.com/name,Values=cluster-name
```

The `cluster.k8s.amazonaws.com/name` tag is automatically added to ENIs created for use by EKS; you just need to supply the correct value (to replace `cluster-name` in the above command). If you're using an automation tool like Pulumi or Terraform, you'll want to be sure you know what the EKS cluster name is; you can assign it, as I did in the code above, or you can look it up.

While I didn't share anything amazingly unique or earth-shattering here, I do hope that this post is helpful to folks. Feel free to find me on various social media platforms---such as [on Twitter][link-4] or [on the Fediverse][link-5]---if you have questions or comments about this post. Constructive feedback is always welcome!

[link-1]: https://docs.aws.amazon.com/eks/latest/userguide/what-is-eks.html
[link-2]: https://www.pulumi.com/
[link-3]: https://go.dev/
[link-4]: https://twitter.com/scott_lowe
[link-5]: https://fosstodon.org/@scottslowe
