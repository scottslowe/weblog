---
author: slowe
categories: Tutorial
comments: true
date: 2020-06-25T10:15:00-07:00
tags:
- Automation
- AWS
- Go
- Pulumi
title: Adopting the Default Route Table of an AWS VPC using Pulumi and Go
url: /2020/06/25/adopting-default-route-table-aws-vpc-using-pulumi-go/
---

Up until now, when I used [Pulumi][link-1] to create infrastructure on AWS, my code would create all-new infrastructure: a new VPC, new subnets, new route tables, new Internet gateway, etc. One thing bothered me, though: when I created a new VPC, that new VPC automatically came with a default route table. My code, however, would create a new route table and then explicitly associate the subnets with that new route table. This seemed less than ideal. (What can I say? I'm a stickler for details.) While building a [Go][link-2]-based replacement for my existing TypeScript code, I found a way to resolve this duplication of resources. In this post, I'll show you how to "adopt" the default route table of an AWS VPC so that you can manage it in your Pulumi code.<!--more-->

Let's assume you are creating a new VPC using code that looks something like this:

```go
vpc, err := ec2.NewVpc(ctx, "testvpc", &ec2.VpcArgs{
	CidrBlock: pulumi.String("10.100.0.0/16"),
	Tags: pulumi.StringMap {
		"Name": pulumi.String("testvpc"),
		k8sTag: pulumi.String("shared"),
	},
})
```

_(Note that this snippet of code doesn't show anything happening with the return values of the `ec2.NewVpc` function, which Go will complain about. Make sure you are either using those values or discarding them. The same goes for the other code snippets found below.)_

If you'd like to now bring the default route table that automatically gets created with a new VPC into Pulumi, then you can do this with this snippet of code:

```go
defrt, err := ec2.NewDefaultRouteTable(ctx, "defrt", &ec2.DefaultRouteTableArgs{
	DefaultRouteTableId: vpc.DefaultRouteTableId,
	Tags: pulumi.StringMap {
		"Name": pulumi.String("defrt"),
		k8sTag: pulumi.String("shared"),
	},
})
```

The [Pulumi documentation for the `ec2.DefaultRouteTable` resource][link-3] indicates that all defined routes will be removed when the default route table. I took this to mean even the route for the VPC's own CIDR (the "10.100.0.0/16" shown in the `ec2.NewVpc` code above), but that wasn't the behavior I observed. Perhaps they only meant separately defined routes? I don't know. The documentation also indicates that you can add routes as part of the adoption. Unfortunately, no example Go code exists, and the syntax/structure for adding routes is completely undocumented. I even tried using the syntax/structure from the `ec2.NewRouteTable` resource, but that didn't work. I never got it to work. Instead, I used a separate `ec2.NewRoute` resource, and simply specified the newly-adopted default route table:

```go
route, err := ec2.NewRoute(ctx, "inet-route", &ec2.RouteArgs {
	RouteTableId: defrt.ID(),
	DestinationCidrBlock: pulumi.String("0.0.0.0/0"),
	GatewayId: gw.ID(),
})
```

The result? A new VPC with only a single route table, and routes for both the VPC's local CIDR as well as to the Internet through an Internet gateway. No explicit subnet-route table associations were necessary (unlike the previous approach), which reduces code to maintain and simplifies the overall code base (as well as simplifies the resources to manage on AWS).

I hope this information is helpful to someone. I'm finding there to be quite a dearth of documentation on using Pulumi with Go, and I hope that my beginner-level posts help alleviate that in some way. If you have questions---or if you have comments on how I can improve my Go code!---feel free to find [me on Twitter][link-5] or on [the Pulumi Slack community][link-4].

**UPDATE 2020-07-01:** I updated the code snippets to use `Pulumi.StringMap` for the tags instead of `Pulumi.Map`. The AWS provider used by Pulumi changed in version 2.11.0 to require string values in maps, hence the need to use `Pulumi.StringMap`.

[link-1]: https://www.pulumi.com/
[link-2]: https://golang.org/
[link-3]: https://www.pulumi.com/docs/reference/pkg/aws/ec2/defaultroutetable/
[link-4]: https://pulumi-community.slack.com/
[link-5]: https://twitter.com/scott_lowe/
