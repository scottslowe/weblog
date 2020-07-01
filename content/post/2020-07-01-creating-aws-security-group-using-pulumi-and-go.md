---
author: slowe
categories: Tutorial
comments: true
date: 2020-07-01T17:20:00-07:00
tags:
- Automation
- AWS
- Go
- Networking
- Pulumi
- Security
title: Creating an AWS Security Group using Pulumi and Go
url: /2020/07/01/creating-aws-security-group-using-pulumi-and-go/
---

In this post, I'm going to share some examples of how to create an AWS security group using [Pulumi][link-1] and [Go][link-2]. I'm sharing these examples because---as of this writing---the Pulumi site does not provide any examples on how this is done using Go. There _are_ examples for the other languages supported by Pulumi, but not for Go. The syntax is, to me at least, somewhat counterintuitive, although I freely admit this could be due to the fact that I am still pretty new to Go and its syntax.<!--more-->

As a framework for providing these examples, I'll use the scenario that I need to create two different security groups. The first security group will allow SSH traffic from the Internet to designated bastion hosts. The second security group will need to allow SSH from those bastion hosts, as well as allow all traffic between/among members of the security group. Between these two groups, I should be able to show enough examples to cover most of the different use cases you'll run into.

Although no example was present for Go when I wrote this article, readers may still find [the API reference for the SecurityGroup resource][link-3] to be useful nevertheless.

First, let's look at the security group to allow SSH traffic to the bastion hosts. Here's a snippet of Go code that will create the desired group:

```go
sshSecGrp, err := ec2.NewSecurityGroup(ctx, "ssh-sg", &ec2.SecurityGroupArgs{
	Name:        pulumi.String("ssh-sg"),
    VpcId:       vpc.ID(),
	Description: pulumi.String("Allows SSH traffic to bastion hosts"),
	Ingress: ec2.SecurityGroupIngressArray{
		ec2.SecurityGroupIngressArgs{
			Protocol:    pulumi.String("tcp"),
			ToPort:      pulumi.Int(22),
			FromPort:    pulumi.Int(22),
			Description: pulumi.String("Allow inbound TCP 22"),
			CidrBlocks:  pulumi.StringArray{pulumi.String("0.0.0.0/0")},
		},
	},
	Egress: ec2.SecurityGroupEgressArray{
		ec2.SecurityGroupEgressArgs{
			Protocol:    pulumi.String("-1"),
			ToPort:      pulumi.Int(0),
			FromPort:    pulumi.Int(0),
			Description: pulumi.String("Allow all outbound traffic"),
			CidrBlocks:  pulumi.StringArray{pulumi.String("0.0.0.0/0")},
		},
	},
})
```

The tricky part, for me, was the syntax around the use of `ec2.SecurityGroupIngressArray` followed by `ec2.SecurityGroupIngressArgs` (although, in retrospect, I shouldn't have been thrown off by this since it follows the same pattern Pulumi uses elsewhere). The `CidrBlocks` parameter is an array (hence the use of `pulumi.StringArray`) with a single entry.

Now let's look at the second security group. This example will demonstrate multiple ingress rules, the use of the `Self` parameter, and referencing a separate security group as a source:

```go
nodeSecGrp, err := ec2.NewSecurityGroup(ctx, "node-sg", &ec2.SecurityGroupArgs{
	Name:        pulumi.String("node-sg"),
	VpcId:       vpc.ID(),
	Description: pulumi.String("Allows traffic between and among nodes"),
	Ingress: ec2.SecurityGroupIngressArray{
		ec2.SecurityGroupIngressArgs{
			Protocol:       pulumi.String("tcp"),
			ToPort:         pulumi.Int(22),
			FromPort:       pulumi.Int(22),
			Description:    pulumi.String("Allow TCP 22 from bastion hosts"),
			SecurityGroups: pulumi.StringArray{sshSecGrp.ID()},
		},
		ec2.SecurityGroupIngressArgs{
			Protocol:    pulumi.String("-1"),
			ToPort:      pulumi.Int(0),
			FromPort:    pulumi.Int(0),
			Description: pulumi.String("Allow all from this security group"),
			Self:        pulumi.Bool(true),
		},
	},
	Egress: ec2.SecurityGroupEgressArray{
		ec2.SecurityGroupEgressArgs{
			Protocol:    pulumi.String("-1"),
			ToPort:      pulumi.Int(0),
			FromPort:    pulumi.Int(0),
			Description: pulumi.String("Allow all outbound traffic"),
			CidrBlocks:  pulumi.StringArray{pulumi.String("0.0.0.0/0")},
		},
	},
})
```

A few things stand out from this second example:

1. To create multiple ingress rules, simply include an `ec2.SecurityGroupIngressArgs` entry for each ingress rule in the `ec2.SecurityGroupIngressArray`. I don't know why I thought it wouldn't be that simple, but it is.
2. Note the use of `Self: pulumi.Bool(true)`; this is what says the source for this ingress rule should be the security group being created.
3. To reference another security group in an ingress rule, use `SecurityGroups: pulumi.StringArray` and reference the `.ID()` of the other security group. The example above uses this to allow SSH from the security group in the first example.

This is all pretty straightforward once you've figured it out (isn't everything?), but without any good examples to help guide the way for new users like myself it can be challenging to get the point where you've figured it out. Hopefully these examples will help in some small way.

If you have any questions, corrections, or comments, please feel free to contact [me on Twitter][link-4], or hit me up in [the Pulumi community Slack][link-5].

[link-1]: https://www.pulumi.com/
[link-2]: https://golang.org/
[link-3]: https://www.pulumi.com/docs/reference/pkg/aws/ec2/securitygroup/
[link-4]: https://twitter.com/scott_lowe
[link-5]: https://pulumi-community.slack.com/
