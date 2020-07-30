---
author: slowe
categories: Tutorial
comments: true
date: 2020-07-29T17:10:00-07:00
tags:
- AWS
- Go
- Pulumi
title: Creating an AWS ELB using Pulumi and Go
url: /2020/07/29/creating-an-aws-elb-using-pulumi-and-go/
---

In case you hadn't noticed, I've been on a bit of a kick with [Pulumi][link-1] and [Go][link-2] recently. There are two reasons for this. First, I have a number of "learning projects" (things that I decide I'd like to try or test) that would benefit greatly from the use of infrastructure as code. Second, I've been working on getting more familiar with Go. The idea of combining both those reasons by using Pulumi with Go seemed natural. Unfortunately, examples of using Pulumi with Go seem to be more limited than examples of using Pulumi with other languages, so in this post I'd like to share how to create an AWS ELB using Pulumi and Go.<!--more-->

Here's the example code:

```go
elb, err := elb.NewLoadBalancer(ctx, "elb", &elb.LoadBalancerArgs{
	NamePrefix:             pulumi.String(baseName),
	CrossZoneLoadBalancing: pulumi.Bool(true),
	AvailabilityZones:      pulumi.StringArray(azNames),
	Instances:              pulumi.StringArray(cpNodeIds),
	SecurityGroups:			pulumi.StringArray{elbSecGrp.ID()},
	HealthCheck: &elb.LoadBalancerHealthCheckArgs{
		HealthyThreshold:   pulumi.Int(3),
		Interval:           pulumi.Int(30),
		Target:             pulumi.String("SSL:6443"),
		UnhealthyThreshold: pulumi.Int(3),
		Timeout:            pulumi.Int(15),
	},
	Listeners: &elb.LoadBalancerListenerArray{
		&elb.LoadBalancerListenerArgs{
			InstancePort:     pulumi.Int(6443),
			InstanceProtocol: pulumi.String("TCP"),
			LbPort:           pulumi.Int(6443),
			LbProtocol:       pulumi.String("TCP"),
		},
	},
	Tags: pulumi.StringMap{
		"Name": pulumi.String(fmt.Sprintf("cp-elb-%s", baseName)),
		k8sTag: pulumi.String("shared"),
	},
})
```

You can probably infer from the code above that this example creates an ELB that listens on TCP port 6443 and forwards to instances on TCP 6443 (and is therefore most likely a load balancer for the control plane of a Kubernetes cluster). Not shown above is the error handling code that would check `err` for a return error.

My use of `pulumi.StringArray` for the "AvailabilityZones" and "Instances" parameters is one way to do it; both `azNames` and `cpNodeIds` are arrays of type `[]pulumi.StringInput`. I took the approach above since I'm collecting information about the various AZs in the `azNames` array using code described [here][xref-1], and I gathered the instance IDs for a group of instances in the `cpNodesId` array.

You could also do something like this, if you wanted to explicitly specify values:

```go
AvailabilityZones: pulumi.StringArray{pulumi.String("us-east-1")}
Instances:         pulumi.StringArray{pulumi.String("i-01234564789")}
```

Note that you should probably use the "Subnets" argument _instead_ of the "AvailabilityZones" argument (you can only use one or the other) if you need to attach the ELB to subnets in a particular VPC.

Getting the syntax for the health check and the listeners was a bit challenging until I reviewed the package documentation for each (see [here][link-3] for the health check arguments and [here][link-4] for the listener arguments). I quickly realized it followed a similar pattern as the ingress and egress rules for an AWS security group (as outlined [in this post][xref-2]).

I hope this example helps. Although I'm not a Go expert (nor am I a Pulumi expert), if you have any questions feel free to reach out to [me on Twitter][link-5]. I also do hang out in [the Pulumi community Slack][link-6], in case you'd like to contact me there.

[link-1]: https://www.pulumi.com/
[link-2]: https://golang.org/
[link-3]: https://pkg.go.dev/github.com/pulumi/pulumi-aws/sdk/v2/go/aws/elb?tab=doc#LoadBalancerHealthCheckArgs
[link-4]: https://pkg.go.dev/github.com/pulumi/pulumi-aws/sdk/v2/go/aws/elb?tab=doc#LoadBalancerListenerArgs
[link-5]: https://twitter.com/scott_lowe
[link-6]: https://pulumi-community.slack.com
[xref-1]: {{< relref "2020-06-24-getting-aws-availability-zones-using-pulumi-and-go.md" >}}
[xref-2]: {{< relref "2020-07-01-creating-aws-security-group-using-pulumi-and-go.md" >}}
