---
author: slowe
categories: Explanation
comments: true
date: 2024-04-17T10:00:00-06:00
tags:
- AWS
- CLI
- Kubernetes
title: Tracking EC2 Instances used by EKS with AWS CLI
url: /2024/04/17/tracking-ec2-instances-used-by-eks-with-aws-cli/
---

As a sort of follow-up to my previous post on using the AWS CLI to track the specific Elastic Network Interfaces (ENIs) used by Amazon Elastic Kubernetes Service (EKS) cluster nodes, this post focuses on the EC2 instances themselves. I feel this is less of a "problem" than tracking ENIs, but I wanted to share this information nevertheless. In this post, I'll show you which AWS CLI command to use to list all the EC2 instances associated with a particular EKS cluster.<!--more-->

If you read [the previous post on tracking ENIs used by EKS][xref-1], you might think that you could use a very similar AWS CLI command (`aws ec2 describe-instances` instead of `aws ec2 describe-network-interfaces`) to track the EC2 instances in a cluster---and you'd be _mostly correct._ Like the ENIs, [EKS][link-1] does add a cluster-specific tag to all EC2 instances in the cluster. However, just to make life interesting, the tag used for EC2 instances is not the same as the tag used for ENIs. (If someone at AWS knows of a technical reason why these tags are different, I'd love to hear it.)

Instead of using the `cluster.k8s.amazonaws.com/name` tag that is used on the ENIs, you'll need to use the `aws:eks:cluster-name` tag instead, like this:

```bash
aws ec2 describe-instances --filters Name=tag:aws:eks:cluster-name,\
Values=<name-of-cluster>
```

Just replace `<name-of-cluster>` in the above command with the name of your EKS cluster, and you're good to go. As I mentioned in the previous post, if you're using an automation tool such as [Pulumi][link-2] or [Terraform][link-3], you may need to explicitly specify the name of the cluster in your code (or look it up after the cluster is created).

I hope this information is useful to folks. If you have questions (or corrections, in the event I have something incorrect here!), please feel free to reach out. You can find me [on Twitter][link-4], on [the Fediverse][link-5], or in a number of different Slack communities. Thanks for reading!

[link-1]: https://docs.aws.amazon.com/eks/latest/userguide/what-is-eks.html
[link-2]: https://www.pulumi.com/
[link-3]: https://www.terraform.io/
[link-4]: https://twitter.com/scott_lowe
[link-5]: https://fosstodon.org/@scottslowe
[xref-1]: {{< relref "2024-04-15-tracking-enis-used-by-eks-with-aws-cli.md" >}}
