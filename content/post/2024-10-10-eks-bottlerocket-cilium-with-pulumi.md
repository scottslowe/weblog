---
author: slowe
categories: Explanation
comments: true
date: 2024-10-10T09:30:00-06:00
tags:
- AWS
- Cilium
- EKS
- Kubernetes
- Pulumi
title: "EKS, Bottlerocket, and Cilium with Pulumi"
url: /2024/10/10/eks-bottlerocket-cilium-with-pulumi/
---

In late 2023, I added some Go code for use with Pulumi to stand up an Amazon Elastic Kubernetes Service (EKS) cluster "from scratch," meaning without using any prebuilt Pulumi components (like the AWSX VPC component or the EKS component). The code is largely illustrative for newer users, written to show how to stitch together all the components needed for an EKS cluster. In this post, I'll show you how to modify that code to use Bottlerocket OS as the node OS for your EKS cluster---and share some information on installing Cilium into (onto?) the cluster.<!--more-->

The example code can be found in [the `pulumi/eks-from-scratch` folder][link-3] in [my "learning-tools" GitHub repository][link-4]. As I mentioned, it's written in [Go][link-5], and the associated README file has full instructions for how to use that code in your own environment. Since the code was intended to be illustrative, I have tried to provide enough comments in the code for readers to be able to decode what's happening without too much difficulty.

To use Bottlerocket OS on the EKS nodes in your cluster, you'll have to modify the `main.go` file. Specifically, changes are needed in the section of code that creates a node group (starting on line 62):

```go
// Create a node group for the EKS cluster
_, err := eks.NewNodeGroup(ctx, "node-group", &eks.NodeGroupArgs{
    ClusterName: testCluster.Name,
    // Additional code omitted for brevity
})
```

Amazon EKS node groups support specifying an AMI type. You'll leverage this functionality to provide a value (found on [this page in the Amazon EKS documentation][link-1]) that instructs EKS to use Bottlerocket OS. You'll supply this value via the `amiType` argument to Pulumi's node group resource (described in more detail on [this page in the Pulumi documentation][link-2]).

If you modify `main.go` to add the `amiType` to the node group definition, then it should look something like this:

```go 
// Create a node group for the EKS cluster
_, err := eks.NewNodeGroup(ctx, "node-group", &eks.NodeGroupArgs{
    amiType: pulumi.String("BOTTLEROCKET_x86_64") // Or BOTTLEROCKET_ARM_64
    ClusterName: testCluster.Name,
    // Additional code omitted for brevity
})
```

Optionally, you could also include the `instanceTypes` argument to the node group definition, which would allow you to control the specific instance types that Amazon EKS would use in the node group.

Once you make that change, just run `pulumi up` and watch Pulumi do its magic. When it's all said and done (it'll take a little bit of time, so go grab coffee or tea while you wait), you'll have an Amazon EKS cluster with nodes running Bottlerocket OS. If you're at all unsure why that's a (generally) good thing, then I encourage you to check out [the Bottlerocket OS web site][link-6] for more details on Bottlerocket OS and its advantages over a traditional general purpose OS.

(I'm also working on how to use [Flatcar Container Linux][link-7] on EKS nodes, but that's proving to be a tad more difficult.)

Once the cluster is up and running, then installing [Cilium][link-9] is a matter of following the instructions, found on [this page in the Cilium documentation][link-8]. However, if I know in advance that I'm planning to deploy Cilium on the cluster, then there are a few additional changes I make to my Pulumi code.

Put these changes in your `main.go` file, starting on line 47 where the EKS cluster itself is defined:

```go
// Create an EKS cluster
testCluster, err := eks.NewCluster(ctx, "test-cluster", &eks.ClusterArgs{
    DefaultAddonsToRemoves: pulumi.StringArray{ // This line is new
        pulumi.String("vpc-cni") // Prevents default CNI from being installed
        pulumi.String("kube-proxy") // If using Cilium kube-proxy replacement
    },
    Name: pulumi.String("testcluster"),
    // Additional code omitted for brevity
})
```

These additional lines (the `DefaultAddonsToRemoves` argument and its parameters) prevent Amazon EKS from installing certain default add-ons. In this example, the AWS VPC CNI and kube-proxy are not installed. This allows you to more easily install Cilium in ENI mode with kube-proxy replacement functionality (you can skip a couple of steps related to cleaning up configurations from these components), but the drawback is that the Pulumi program takes _far_ longer to run since the EKS nodes never go into a ready state. For me, the tradeoff is worth it; however, you'll need to decide for yourself which approach works best in your environment and for your situation.

That's all I have to share this time around! While I didn't share anything revolutionary or incredibly insightful, I hope that sharing this information in the context of example code has been useful. If you have any questions, I'd certainly love to hear from you; feel free to reach out to me [on Twitter][link-10], on [the Fediverse][link-11], in [the Cilium Slack instance][link-12], or via e-mail. Thanks for reading!

[link-1]: https://docs.aws.amazon.com/eks/latest/APIReference/API_Nodegroup.html#AmazonEKS-Type-Nodegroup-amiType
[link-2]: https://www.pulumi.com/registry/packages/aws/api-docs/eks/nodegroup/#inputs
[link-3]: https://github.com/scottslowe/learning-tools/tree/main/pulumi/eks-from-scratch
[link-4]: https://github.com/scottslowe/learning-tools/
[link-5]: https://go.dev
[link-6]: https://bottlerocket.dev
[link-7]: https://www.flatcar.org/
[link-8]: https://docs.cilium.io/en/stable/installation/k8s-install-helm/
[link-9]: https://cilium.io/
[link-10]: https://twitter.com/scott_lowe
[link-11]: https://fosstodon.org/@scottslowe
[link-12]: https://slack.cilium.io/
