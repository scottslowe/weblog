---
author: slowe
categories: Tutorial
comments: true
date: 2019-08-21T21:00:00Z
tags:
- AWS
- CLI
- Pulumi
- TypeScript
title: Creating Tagged Subnets Across AWS AZs Using Pulumi
url: /2019/08/21/creating-tagged-subnets-across-aws-azs-using-pulumi/
---

As I mentioned back in May in [this post on creating a sandbox for learning Pulumi][xref-1], I've started using [Pulumi][link-1] more and more of my infrastructure-as-code needs. I did switch from JavaScript to TypeScript (which I know compiles to JavaScript on the back-end, but the strong typing helps a new programmer like me). Recently I had a need to create some resources in AWS using Pulumi, and---for reasons I'll explain shortly---many of the "canned" Pulumi examples didn't cut it for my use case. In this post, I'll share how I created tagged subnets across AWS availability zones (AZs) using Pulumi.<!--more-->

In this particular case, I was using Pulumi to create all the infrastructure necessary to spin up an AWS-integrated [Kubernetes][link-6] cluster. That included a new VPC, subnets in the different AZs for that region, an Internet gateway, route tables and route table associations, security groups, an ELB for the control plane, and EC2 instances. As I've outlined in my latest post on [setting up an AWS-integrated Kubernetes 1.15 cluster using `kubeadm`][xref-2], these resources on AWS require specific AWS tags to be assigned in order for the AWS cloud provider to work.

As I started working on this, I found examples of Pulumi TypeScript code that could do _part_ of what I needed, but not all of it:

* If you're using the `awsx` module with Pulumi (part of their "Crosswalk" stuff) [to create a VPC][link-2], it can handle creating subnets across multiple AZs automatically for you. Users have the ability to fine-tune some of the settings, but---here's the kicker---tags you specify at the VPC level don't get propagated. (There's [an issue][link-3] open for this.)
* If you build it all manually, then you also have to manually deal with figuring out how many AZs are in a region, and how to loop through the AZs to create subnets in each one (which is stuff the `awsx` module handles for you).

Since I needed the tags---the AWS cloud provider for Kubernetes won't work otherwise---I set out to take the second path (building it all out manually). I'm going to share the TypeScript code I used---and am still using---so that others who may find themselves in the same boat have an example upon which to base their code.

(I'm a relative newcomer to TypeScript, so be gentle!)

First, I needed to figure out how many AZs were in the given region. This snippet of code returned both the list of AZ names as well as the total number of AZs in the region:

```typescript
const rawAzInfo = aws.getAvailabilityZones({
    state: "available",
});
let azNames: Array<string> = rawAzInfo.names;
let numberOfAZs: number = azNames.length;
```

I gathered both the list of AZ names as well as the overall count because in the snippet of code for creating subnets, I'll need the AZ names.

Next, I created a VPC (this part is straightforward and reasonably well-documented):

```typescript
const vpc = new aws.ec2.Vpc("k8s-vpc", {
    cidrBlock: "10.1.0.0/16",
    enableDnsHostnames: true,
    enableDnsSupport: true,
    tags: {
        Name: "k8s-vpc",
        [k8sTagName]: "shared",
    },
});
```

The one part here that wasn't immediately obvious to me was the `[k8sTagName]` syntax for referencing a variable named `k8sTagName`. This allow me to declare the tag (i.e., "kubernetes.io/cluster/cluster-name") once in a variable instead of multiple times in the code.

Now we get to the more interesting stuff---iterating through the list of AZs to create a subnet in each. After stumbling around for a bit, I finally arrived here:

```typescript
let subnets = [];
for (let i = 0; i < numberOfAZs; i++) {
    let subnetAddr: number = i*16;
    let netAddr: string = "10.1.";
    let cidrSubnet: string = netAddr.concat(String(subnetAddr), ".0/20");
    subnets.push(new aws.ec2.Subnet(`subnet-${i+1}`, {
        availabilityZone: azNames[i],
        cidrBlock: cidrSubnet,
        mapPublicIpOnLaunch: true,
        vpcId: vpc.id,
        tags: {
            Name: `subnet-${i+1}`,
            [k8sTagName]: "shared",
        },
    }));
};
```

So what does this code do?

1. First, it creates an array to hold the `aws.ec2.Subnet` objects I'm going to create in the loop.
2. Next, it jumps into a `for` loop that iterates over the total number of AZs in the region (hence why I collected that value earlier).
3. For each iteration, the code calculates a subnet address (as a multiple of 16), and then combines that subnet address with the network address and CIDR mask. Thus, the CIDR block for the first subnet is "10.1.0.0/20", the second is "10.1.16.0/20", and so on.
4. Finally, for each iteration, the `push` method adds ("pushes") a new `aws.ec2.Subnet` object into the array with the appropriate properties (the VPC ID for the VPC created earlier, the calculated CIDR block, any other settings, and the necessary tags). Note the `subnet-${i+1}` syntax to calculate names like "subnet-1", "subnet-2", etc.

After this, the majority of the rest of the code is pretty straightforward (I won't go into detail in this post since [the examples][link-5] and [Pulumi API reference][link-4] pretty much capture most everything needed):

* Create an Internet gateway in the VPC and tag it.
* Create a route table for the Internet gateway and tag it.
* Use a similar array and `for` loop construct to iterate across the subnets and associate each subnet (referenceable by `subnets[i]`, assuming `i` is the loop variable) with the route table.

At this point, Pulumi has now created a VPC, subnets in each AZ, an Internet gateway, a route table to point to the Internet gateway, and a route table association linking each subnet to the route table. All you need now are security groups and EC2 instances, and you're off to the races! All of the things that _can_ be tagged _are_ tagged, which means infrastructure provisioned with this code is ready for use with Kubernetes.

It's more than fair to say that some of this is just me learning TypeScript; I can't dispute that at all. If I'm searching for resources on how to do something, though, that's probably a reasonable indicator that other folks are also looking for resources and examples, and why I wanted to publish this post.

I hope that this information is useful to someone; feel free to [hit me up on Twitter][link-99] if you have any questions, comments, corrections, or suggestions.

[link-1]: https://www.pulumi.com/
[link-2]: https://www.pulumi.com/docs/reference/crosswalk/aws/vpc/
[link-3]: https://github.com/pulumi/pulumi-awsx/issues/383
[link-4]: https://www.pulumi.com/docs/reference/pkg/
[link-5]: https://github.com/pulumi/examples
[link-6]: https://kubernetes.io/
[link-99]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2019-05-05-a-sandbox-for-learning-pulumi.md" >}}
[xref-2]: {{< relref "2019-08-14-setting-up-aws-integrated-kubernetes-115-cluster-kubeadm.md" >}}
