---
author: slowe
categories: Tutorial
comments: true
date: 2021-08-26T09:25:00-06:00
tags:
- AWS
- Networking
- Go
- Pulumi
title: Establishing VPC Peering with Pulumi and Go
url: /2021/08/26/establishing-vpc-peering-with-pulumi-and-go/
---

I use [Pulumi][link-2] to manage my lab infrastructure on [AWS][link-5] (I shared some of the details in [this April 2020 blog post][link-1] published on the Pulumi site). Originally I started with [TypeScript][link-3], but later switched to [Go][link-4]. Recently I had a need to add some VPC peering relationships to my lab configuration. I was concerned that this may pose some problems---due entirely to the way I structure my Pulumi projects and stacks---but as it turned out it was more straightforward than I expected. In this post, I'll share some example code and explain what I learned in the process of writing it.<!--more-->

## Some Background

First, let me share some background on how I structure my Pulumi projects and stacks.

It all starts with a Pulumi project that manages my base AWS infrastructure---VPC, subnets, route tables and routes, Internet gateways, NAT gateways, etc. I use a separate stack in this project for each region where I need base infrastructure.

All other projects build on "top" of this base project, referencing the resources created by the base project in order to create their own resources. Referencing the resources created by the base project is accomplished via a Pulumi StackReference.

In my particular instance, I wanted to create a VPC peering relationship between two VPC in different regions, i.e., between two stacks of the base infrastructure project. However, I had some questions/concerns about how to do this:

* I potentially _could_ have added VPC peering into the base infrastructure project, since the VPC peering requester and the VPC peering accepter are separate resources.
* However, I wanted the flexibility to _optionally_ create a peering relationship, which would not have been possible if I bundled it into the base infrastructure project (without building in some branching logic to make it optional).
* And yet, if I used a separate project (which affords me the flexibility of optionally adding a peering relationship), then how would that separate project add things to the base project that were necessary for the peering relationship to work, like routes and security group rules?

Although the Pulumi documentation has improved and continues to improve, there was no documentation or articles that really addressed these questions/concerns. I will provide a shout-out to Itay from [the Pulumi community Slack][link-7], who took some time to share their experience with VPC peering (it was very useful).

## Establishing VPC Peering

To establish a VPC peering relationship, a few different resources are needed (note that each of these is considered its own independent Pulumi resource, not a property of another resource):

1. The VPC peering connection, which references the VPC IDs on both sides
2. The VPC peering connector accepter, which references the VPC peering connection
3. New routes to direct traffic between the two VPC CIDRs (these wouldn't already exist because these routes need to reference the VPC peering connection in order to direct traffic appropriately)
4. New security group rules to allow traffic from the peer VPC CIDR (unless this traffic is already allowed)

Let's look at some code. Before I can create any of these resources, I need to pull some information from the base infrastructure project stacks via StackReferences. Assuming the StackReferences are named `srcStackRef` and `dstStackRef`, then I could pull the corresponding (exported) information like this:

```go
srcPrivateRouteTbl := srcStackRef.GetIDOutput(pulumi.String("privRouteTableId"))
srcVpcId := srcStackRef.GetIDOutput(pulumi.String("vpcId"))
srcNodeSecGrpId := srcStackRef.GetIDOutput(pulumi.String("nodeSecGrpId"))
dstPrivateRouteTbl := dstStackRef.GetIDOutput(pulumi.String("privRouteTableId"))
dstVpcId := dstStackRef.GetIDOutput(pulumi.String("vpcId"))
dstNodeSecGrpId := dstStackRef.GetIDOutput(pulumi.String("nodeSecGrpId"))
```

That last note is important: the information I want to pull using a StackReference must be exported via `ctx.Export()` in the base infrastructure project. Fortunately, I'd exported just about everything, so no changes to the base infrastructure project were needed.

Next, I needed to set up a new AWS provider. Since creating a VPC peering relationship across regions means creating resources in two different regions (in my use case, at least), a new (additional) AWS provider to handle the second region is needed:

```go
dstProvider, err := aws.NewProvider(ctx, "dstProvider", &aws.ProviderArgs{
	Region: pulumi.String(dstVpcRegion),
})
```

Armed with the second provider and the information from the base infrastructure project stacks (the names of which are parameterized to make the code more reusable), I can proceed with creating the VPC peering connection and VPC peering connection accepter:

```go
peerConn, err := ec2.NewVpcPeeringConnection(ctx, "peering-connection", &ec2.VpcPeeringConnectionArgs{
	PeerRegion: pulumi.String(dstVpcRegion),
	PeerVpcId:  dstVpcId,
	VpcId:      srcVpcId,
})

_, err = ec2.NewVpcPeeringConnectionAccepter(ctx, "peering-acceptor", &ec2.VpcPeeringConnectionAccepterArgs{
	VpcPeeringConnectionId: peerConn.ID(),
	AutoAccept:             pulumi.Bool(true),
}, pulumi.Provider(dstProvider))
```

At this point, the relationship is created, but no traffic will pass between the VPCs (there's no route and the traffic wouldn't be allowed by my security groups anyway). Now we start to get into the area where most of my questions/concerns were centered: how was this third project going to be able to modify things that sat inside the base infrastructure project, like route tables and security groups? Using a third project---as opposed to building the peering into the base infrastructure project---seemed like the best/right approach. Would it work?

As it turns out, yes, it _does_ work! I had been thinking too "atomically," thinking of the route table and the security group as singular entities. In reality, they are not; we add routes to a route table via a route table association, and both routes and route table associations are separate resources from the route table itself. Similarly, security group rules can exist as an independent resource, referencing only the ID of the security group in which those rules should be included. This was a key expansion of my understanding.

Here's the code to create the new routes (the VPC CIDRs are parameterized):

```go
_, err = ec2.NewRoute(ctx, "src-peer-route", &ec2.RouteArgs{
	RouteTableId:           srcPrivateRouteTbl,
	DestinationCidrBlock:   pulumi.String(netAddrMap[dstVpcRegion]),
	VpcPeeringConnectionId: peerConn.ID(),
})

_, err = ec2.NewRoute(ctx, "dst-peer-route", &ec2.RouteArgs{
	RouteTableId:           dstPrivateRouteTbl,
	DestinationCidrBlock:   pulumi.String(netAddrMap[srcVpcRegion]),
	VpcPeeringConnectionId: peerConn.ID(),
}, pulumi.Provider(dstProvider))
```

You can see that I needed only to reference the route table ID in order to create the route (and the peering connection ID, of course, but that was created in this same project).

Similarly, referencing the security group ID gained via a StackReference to the base infrastructure project stacks allowed me to insert a security group rule to allow the traffic:

```go
_, err = ec2.NewSecurityGroupRule(ctx, "src-peer-cidr", &ec2.SecurityGroupRuleArgs{
	Type:            pulumi.String("ingress"),
	FromPort:        pulumi.Int(0),
	ToPort:          pulumi.Int(65535),
	Protocol:        pulumi.String("all"),
	CidrBlocks:      pulumi.StringArray{pulumi.String(dstVpcCidr)},
	SecurityGroupId: srcNodeSecGrpId,
})

_, err = ec2.NewSecurityGroupRule(ctx, "dst-peer-cidr", &ec2.SecurityGroupRuleArgs{
	Type:            pulumi.String("ingress"),
	FromPort:        pulumi.Int(0),
	ToPort:          pulumi.Int(65535),
	Protocol:        pulumi.String("all"),
	CidrBlocks:      pulumi.StringArray{pulumi.String(srcVpcCidr)},
	SecurityGroupId: dstNodeSecGrpId,
}, pulumi.Provider(dstProvider))
```

In all of the above examples, please note that I've omitted code to handle the value of `err` and to return errors; you'd want to add that yourself before you can use the code.

Running `pulumi up` was successful (no errors, first try!), and a quick check of connectivity showed that my workloads were able to communicate across the VPC peering relationship. Success!

## Lesson Learned

The key thing I gained from working on this was a better understanding of the relationship between things like route tables and routes, or between security group rules and security groups. Being able to separate the management of routes in a table or rules in a security group into separate projects is very useful, and makes it much easier to "layer" projects and stacks.

I hope this post is helpful. If you have any questions, or if you have corrections or suggestions for improving the post, feel free to reach out to me. You can easily find [me on Twitter][link-6], and I also hang out in [the Pulumi Slack community][link-7].

[link-1]: https://www.pulumi.com/blog/supporting-kubernetes-with-pulumi/
[link-2]: https://www.pulumi.com/
[link-3]: https://www.typescriptlang.org/
[link-4]: https://golang.org/
[link-5]: https://aws.amazon.com/
[link-6]: https://twitter.com/scott_lowe
[link-7]: https://pulumi-community.slack.com
