---
author: slowe
categories: Tutorial
comments: true
date: 2020-06-24T09:45:00-07:00
tags:
- AWS
- Go
- Pulumi
title: Getting AWS Availability Zones using Pulumi and Go
url: /2020/06/24/getting-aws-availability-zones-using-pulumi-and-go/
---

I've written several different articles on [Pulumi][link-1] (take a look at [all articles tagged "Pulumi"][link-2]), the infrastructure-as-code tool that allows users to define their infrastructure using a general-purpose programming language instead of a domain-specific language (DSL). Thus far, my work with Pulumi has leveraged TypeScript, but moving forward I'm going to start sharing more Pulumi code written using [Go][link-3]. In this post, I'll share how to use Pulumi and Go to get a list of Availability Zones (AZs) from a particular region in AWS.<!--more-->

Before I proceed, I feel like it is important to provide the disclaimer that I'm new to Go (and therefore still learning). There are probably better ways of doing what I'm doing here, and so I welcome all constructive feedback on how I can improve.

With that disclaimer out of the way, allow me to first provide a small bit of context around this code. When I'm using Pulumi to manage infrastructure on AWS, I like to try to keep things as region-independent as possible. Therefore, I try to avoid hard-coding things like the number of AZs or the AZ names, and prefer to gather that information dynamically---which is what this code does.

Here's the Go code I concocted:

```go
package main

import (
	"github.com/pulumi/pulumi-aws/sdk/v2/go/aws"
	"github.com/pulumi/pulumi/sdk/v2/go/pulumi"
)

func main() {
	pulumi.Run(func(ctx *pulumi.Context) error {
		// Look up Availability Zone (AZ) information for configured region
		desiredAzState := "available"

		rawAzInfo, err := aws.GetAvailabilityZones(ctx, &aws.GetAvailabilityZonesArgs{
			State: &desiredAzState,
		})
		if err != nil {
			return err
		}

		// Determine how many AZs are present
		numOfAZs := len(rawAzInfo.Names)

		// Build a list of AZ names
		azNames := []string{}
		for idx := 0; idx < numOfAZs; idx++ {
			azNames = append(azNames, rawAzInfo.Names[idx])
		}

		return nil
	})
}
```

The code above assumes that you've defined the desired AWS region using `pulumi config set aws:region <region>` before running it. I'm sure I could add some code to check for that (which I will very likely do in the near future). Otherwise, this code has no other dependencies and will gather the total number of AZs in a region (stored in `numOfAZs`) as well as a list of AZ names (stored in the `azNames` slice). Using the number of AZs and the list of AZ names, you could then use that information to create subnets in each AZ, distribute instances across AZs, or similar.

This code doesn't define any output, but it would be reasonably straightforward to add `ctx.Export` statements to add some output (which would be useful/necessary if you needed to consume information from this stack in another stack via a StackReference).

## How I Tested

I tested this Go code with Pulumi 2.4.0 on a Debian 10.4 VM. The VM had Go 1.14.4, Pulumi, and the AWS CLI installed and configured.

There aren't a lot of examples of using Go with Pulumi (my colleague Leon Stigter is one of the few folks I've seen writing about using Go with Pulumi, check [his website][link-5]), so I hope that sharing this will help others who want to use Go with Pulumi. If you have any questions, feel free to contact [me on Twitter][link-99] or find me in [the Pulumi Slack community][link-4].

[link-1]: https://www.pulumi.com/
[link-2]: /tags/pulumi/
[link-3]: https://golang.org/
[link-4]: https://pulumi-community.slack.com
[link-5]: https://retgits.com/
[link-99]: https://twitter.com/scott_lowe
