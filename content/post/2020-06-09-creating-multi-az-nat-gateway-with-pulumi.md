---
author: slowe
categories: Tutorial
comments: true
date: 2020-06-09T13:55:00-07:00
tags:
- AWS
- Networking
- Pulumi
- Automation
- TypeScript
title: Creating a Multi-AZ NAT Gateway with Pulumi
url: /2020/06/09/creating-multi-az-nat-gateway-with-pulumi/
---

I recently had a need to test a configuration involving the use of a single NAT Gateway servicing multiple private subnets across multiple availability zones (AZs) within a single VPC. While there are notable caveats with such a design (see the "Caveats" section at the bottom of this article), it could make sense in some use cases. In this post, I'll show you how I used [TypeScript][link-4] with [Pulumi][link-2] to automate the creation of this design.<!--more-->

For the most part, if you're familiar with Pulumi and using TypeScript with Pulumi, this will be pretty straightforward. The code I'll show you makes a couple assumptions:

1. It assumes you've already created the VPC and the subnets earlier in the code. I'll reference the VPC object as `vpc`.
2. I'll assume you've already created subnets in said VPC, and that the subnet-to-AZ ratio is 1:1 (exactly one subnet of each type---public or private---in each AZ). The code will reference the subnet IDs as `pubSubnetIds` (for public subnets) or `privSubnetIds` (for private subnets). (How to create the subnets and capture the list of IDs is left as an exercise for the reader. If you'd be interested in seeing how I do it, let me know.)

The first step is creating an Elastic IP to assign to the NAT Gateway:

```typescript
let natGwEip = new aws.ec2.Eip("natgw-eip", {
    vpc: true,
});
```

Next, create the NAT Gateway itself:

```typescript
let natGw = new aws.ec2.NatGateway("natgw", {
    allocationId: natGwEip.id,
    subnetId: pubSubnetIds[0],
});
```

This creates the NAT Gateway and attaches it to the first public subnet in the list of public subnet IDs stored in `pubSubnetIds`. You could, obviously, attach it to a different public subnet as needed.

Next, create a route table that will provide Internet access via the NAT Gateway:

```typescript
let natGwRoute = new aws.ec2.RouteTable("natgw-route", {
    vpcId: vpc.id,
    routes: [
        { cidrBlock: "0.0.0.0/0", natGatewayId: natGw.id },
    ],
});
```

Finally, associate this route table with the private subnets. The code below iterates across the number of AZs in the specified region (again, I'll leave exactly how to gather that information as an exercise for readers; let me know if you are interested in seeing that portion of code) and make a route table association for each subnet/AZ:

```typescript
let privRtAssoc = [];
for (let i = 0; i < numberOfAZs; i++) {
    privRtAssoc.push(new aws.ec2.RouteTableAssociation(`priv-rta-${i+1}`, {
        routeTableId: natGwRoute.id,
        subnetId: privSubnetIds[i],
    }));
};
```

When you run `pulumi up` to create this infrastructure, you'll end up with a single NAT Gateway, attached to one public subnet and multiple private subnets. Instances in each private subnet will be able to access Internet-based resources via the single NAT Gateway.

## Caveats

It's important to note that outbound traffic will have to cross AZ boundaries in order to egress onto the Internet, and therefore will be subject to cross-AZ data transfer fees (see [this article][link-1] by Corey Quinn for more details). Be sure to understand your traffic patterns and understand the impact of this sort of design on your traffic patterns and your billing before going down this path. (Of course, that's good advice for _any_ design.) [This article][link-5] also has some good information on reducing data transfer charges for your NAT Gateway(s) (thanks to reader Amey Bhide for the link!).

Feel free to connect with [me on Twitter][link-99], or find me in any one of a number of Slack communities (including [the Pulumi Slack community][link-3]), if you have questions, comments, or suggestions for improvement.

[link-1]: https://www.lastweekinaws.com/blog/aws-cross-az-data-transfer-costs-more-than-aws-says/
[link-2]: https://www.pulumi.com/
[link-3]: https://pulumi-community.slack.com
[link-4]: https://www.typescriptlang.org/
[link-5]: https://aws.amazon.com/premiumsupport/knowledge-center/vpc-reduce-nat-gateway-transfer-costs/
[link-99]: https://twitter.com/scott_lowe
