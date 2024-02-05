---
author: slowe
categories: Explanation
comments: true
date: 2024-02-05T09:30:00-06:00
tags:
- AWS
- Go
- IaC
- NAT
- Networking
- Pulumi
title: Using NAT Instances on AWS with Pulumi
url: /2024/02/05/using-nat-instances-on-aws-with-pulumi/
---

For folks using AWS in their day-to-day jobs, it comes as no secret that AWS' Managed NAT Gateway---responsible for providing outbound Internet connectivity to otherwise private subnets---is an expensive proposition. While the primary concern for large organizations is the data processing fee, the concern for smaller organizations or folks like me who run a cloud-based lab instead of a hardware-based home lab is the per-hour cost. In this post, I'll show you how to use Pulumi to use a NAT instance for outbound Internet connectivity instead of a Managed NAT Gateway.<!--more-->

For a bit more about why Managed NAT Gateways aren't ideal for larger organizations, I'd recommend [this article by Corey Quinn][link-4]. For smaller organizations or cloud-based labs, data processing fees probably aren't the main concern (although I could be wrong); it would be the ~$32/mo per Managed NAT Gateway. Since many tools configure a Managed NAT Gateway per availability zone, now you're talking more like $96/mo---and you haven't even spun up any real workloads yet! Running your own NAT instance can dramatically reduce but not eliminate this expense.

Now that I've established _why_ running a NAT instance can be beneficial, let's review what you'll need to have installed in order to follow along with (or use) what I'll show you in this post:

1. I'm automating the entire process with [Pulumi][link-5], so you'll want to have the Pulumi CLI installed. (Installation instructions are [here][link-6].)
1. I write my Pulumi using [Go][link-7], so you'd need Go installed. (Installation instructions are [here][link-8].)
1. A typical EC2 AMI isn't pre-configured for NAT, so you'll need _either_ a configuration mechanism for setting that up (like [Ansible][link-9] and an associated playbook) _or_ a preconfigured AMI. I chose to go the latter route and am using the excellent fck-nat AMI (check out [the website][link-1] and the associated [GitHub repository][link-2]).

I'll walk through select pieces of the code below to explain what's being provisioned or configured. For your reference, the full code is found in [my GitHub "learning-tools" repository][link-3], in the `aws/nat-instance-pulumi` folder.

## Setting up the VPC and Subnets

All the Pulumi code for setting up the VPC and subnets is separated into a file named `vpc.go`, and is invoked from `main.go` through a function named `buildInfrastructure`. At a high-level, the `buildInfrastructure` function does the following things:

* It gets the number of availability zones (AZs) and the names of the zones, and stores that information for later use.
* It builds a VPC with a preconfigured CIDR block. (In most of my Pulumi programs I make this a configuration value, but in this particular case it's hard-coded. There's no reason for that other than my own lack of time.)
* It creates a public subnet in each of the AZs.
* It handles the routing configuration for the public subnets (creates an Internet Gateway, creates a route table, creates an outbound route via the gateway, and links the public subnets to the route table).
* It creates a private subnet in each of the AZs.
* It creates a route table for the private subnets and links the private subnets to the route table, but _does not create a route._

All said, that's about 150 lines of code. You might wonder why I didn't use Pulumi's AWSX (Crosswalk for AWS) component for a VPC, which allows users to do _almost_ the same thing in about 10 lines of code. That would be an excellent question! Currently, the AWSX VPC component [doesn't currently expose the route table IDs][link-10], which are needed so that I can add a route of my own creation. The AWSX VPC component is outstanding otherwise; if you can use it for your use case, I generally recommend it.

## Setting up the NAT Infrastructure

Now the program moves on to creating the necessary NAT infrastructure. This code is split into a separate file named `nat.go` and invoked from `main.go` via the `buildNat` function.

This code is reasonably straightforward:

* It creates a security group to allow traffic to move through the NAT instance.
* It dynamically looks up the AMI ID for the fck-nat instance.
* It launches an EC2 instance (a "tg4.nano" is sufficient to handle Gbps-level traffic) using the fck-nat AMI.
* Once the EC2 instance is launched, it adds a route to the private subnet route table that directs outbound traffic for the private subnets through the EC2 instance. (We couldn't do that earlier because we needed the interface ID associated with the EC2 instance.)

## Finishing the Final Touches

For your own architecture implementation, you could stop there, but my code continues on so that there's a way to test that the NAT instance is working as expected. All of this code is found in `main.go`.

Before `main.go` invokes the `buildInfrastructure` and `buildNat` functions, it first creates an SSH key and an associated AWS key pair. It passes the key pair name to the `buildNat` function so that the fck-nat instance is configured with the SSH key. This allows you to SSH into the NAT instance with the user "ec2-user" and the associated private key (which you can get from Pulumi using `pulumi stack output`).

After invoking `buildInfrastructure` and `buildNat`, the Pulumi program goes on to create an EC2 instance (based on a dynamically-obtained AMI ID) in one of the private subnets and a security group to allow SSH traffic to that instance. This allows you to test that the fck-nat instance is both a) working properly as an SSH bastion host, and b) working properly as a NAT instance.

Congratulations! You have now reduced your NAT costs to about 1/10th the cost of a Managed NAT Gateway.

## Caveats

This code isn't necessarily intended for commercial production use, as there are a number of caveats with the architecture it creates:

* There is only a single NAT instance for all AZs. If that AZ fails, then outbound traffic from all private subnets in other AZs is also down.
* There is only a single NAT instance. If the NAT instance fails, then...well, you get the idea.

The fck-nat AMI has some functionality to help address some of these caveats, so I encourage you to review [the website][link-1] for more information. I'll leave updating this code to support these features as an exercise for the reader. (Feel free to submit one or more PRs if you are so inclined.)

## Additional Resources

To get access to the full Pulumi program, see [my GitHub "learning-tools" repository][link-3] in the `aws/nat-instance-pulumi` folder. If you have questions about the code or about Pulumi, feel free to join [the Pulumi Community Slack][link-11], where I and other Pulumi enthusiasts and experts hang out. You're also welcome to find me online; I am available [on Twitter][link-12], [on the Fediverse][link-13], and in various other Slack communities. I'd be more than happy to hear from readers with questions or feedback on this or any article on my site. Thanks for reading!

[link-1]: https://fck-nat.dev/stable/
[link-2]: https://github.com/AndrewGuenther/fck-nat
[link-3]: https://github.com/scottslowe/learning-tools
[link-4]: https://www.lastweekinaws.com/blog/the-aws-managed-nat-gateway-is-unpleasant-and-not-recommended/
[link-5]: https://www.pulumi.com/
[link-6]: https://www.pulumi.com/install/
[link-7]: https://go.dev/
[link-8]: https://go.dev/doc/install/
[link-9]: https://www.ansible.com/
[link-10]: https://github.com/pulumi/pulumi-awsx/pull/885
[link-11]: https://slack.pulumi.com/
[link-12]: https://twitter.com/scott_lowe
[link-13]: https://fosstodon.org/@scottslowe
