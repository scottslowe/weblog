---
author: slowe
categories: Tutorial
comments: true
date: 2022-07-27T16:00:00-06:00
tags:
- AWS
- Go
- Networking
- Pulumi
title: Using Default AWS Resources with Pulumi
url: /2022/07/27/using-default-aws-resources-with-pulumi/
---

Per [the AWS documentation][link-1] (although I'm sure there are exceptions), when you start using AWS you are given some automatically-created resources: a default VPC that contains public subnets in each availability zone in the region along with an Internet gateway and settings to enable DNS resolution. Most of the infrastructure-as-code tutorials that I've seen start with creating a VPC and subnets and gateway, but what if you wanted to use these default resources instead? I wasn't really able to find a good walkthrough on how to do this, so this post provides some sample [Go][link-2] code you can use with [Pulumi][link-3] to identify these default AWS resources and use them.<!--more-->

I'll approach this from the perspective of wanting to launch an EC2 instance in the default infrastructure that AWS provides for you in a region. To launch an EC2 instance using Pulumi (and most other infrastructure-as-code tools), there are several pieces of information you need:

1. An AMI ID
2. The instance type
3. The name of an SSH keypair that's been uploaded to/created in AWS
4. A subnet ID
5. A security group ID

The first three are probably things you'll want to parameterize (i.e., make it possible for you to pass values in for your code to use). You can use Pulumi to look up the remaining values, so let's see how to do that.

The first step is to look up the VPC itself. It _might_ be possible for you to skip this step, but I like to include it for the sake of completeness. The Pulumi Go SDK has a function called "LookupVpc" that does exactly what you need it to do:

```go
// Look up the default VPC
isDefault := true
desiredState := "available"
vpc, err := ec2.LookupVpc(ctx, &ec2.LookupVpcArgs{
	Default: &isDefault,
	State:   &desiredState,
    },
)
```

Perfect; now you have the default VPC for your account in whatever region you're using. Next, you need to look up the availability zones (AZs) in your selected region, because you'll need that information when you go to find the subnets. This is a bit more complicated than just using the "GetAvailabilityZones" function; because the number of AZs may vary from region to region, some additional code is needed:

```go
// Look up availability zones in the desired region
rawAzInfo, err := aws.GetAvailabilityZones(ctx, &aws.GetAvailabilityZonesArgs{
	State: &desiredState,
})
// Determine how many AZs are present
numOfAZs := len(rawAzInfo.Names)
ctx.Export("numOfAZs", pulumi.Int(numOfAZs))
// Build a list of AZ names
azNames := make([]string, numOfAZs)
for i := 0; i < numOfAZs; i++ {
	azNames[i] = rawAzInfo.Names[i]
}
```

Now you have the VPC and information about the AZs (including the number of AZs and the names of each AZ). Next, you can use the "LookupSubnet" function to look up the subnets in each AZ:

```go
// Iterate through the AZs to discover subnets
pubSubnetIds := make([]pulumi.StringInput, numOfAZs)
for i := 0; i < numOfAZs; i++ {
	selectedAz := azNames[i]
	azDefault := true
	subnet, err := ec2.LookupSubnet(ctx, &ec2.LookupSubnetArgs{
		AvailabilityZone: &selectedAz,
		DefaultForAz:     &azDefault,
		VpcId:            &vpc.Id,
	})
	pubSubnetIds[i] = pulumi.String(subnet.Id)
}
```

This code iterates over the list of AZs, and for each AZ looks up the subnet that is in the default VPC and is considered the "default" subnet for that AZ. It stores this list of subnets in the array named "pubSubnetIds", which you'll use later.

Only the security group remains before you have all the information you need to launch an EC2 instance:

```go
// Identify default SG
defaultSgName := "default"
sg, err := ec2.LookupSecurityGroup(ctx, &ec2.LookupSecurityGroupArgs{
	Name: &defaultSgName,
})
```

And with all the information in hand, you're now ready to launch your EC2 instance:

```go
// Launch an instance
instance, err := ec2.NewInstance(ctx, "instance", &ec2.InstanceArgs{
	Ami:                      pulumi.String("ami-123456789"),
	InstanceType:             pulumi.String("t3.large"),
	KeyName:                  pulumi.String("my-keypair-name"),
	SubnetId:                 pubSubnetIds[0],
	VpcSecurityGroupIds:      pulumi.StringArray{sg.ID()},
	Tags: pulumi.StringMap{
		"Name":       pulumi.String("my-instance-name"),
	},
})
```

Obviously, this code doesn't show parameterizing the AMI ID, instance type, or key pair name; I'll leave that as an exercise for the reader. (Or, if you _really_ want to see that information, let me know and I'll add it in another blog post.) The code snippets above also don't show any error handling or such, which would be necessary (the Go compiler will complain if you don't reference the `err` variable populated in these code snippets). However, you should be able to assemble this into a working Pulumi program without too much difficulty.

For those that would like a bit more assistance in putting all this together in a working example, have a look in the `pulumi/default-aws-infra` folder of [my GitHub "learning-tools" repository][link-4]. There you'll find a complete, working Pulumi program incorporating all the code shown above.

I hope this is helpful to folks out there. If you find an error in this post, please let me know so that I can fix it! You can reach [me on Twitter][link-5], and I also lurk in [the Pulumi community Slack instance][link-6]. Alternately, if you just feel like reaching out and saying hi, I'd love that, too!

[link-1]: https://docs.aws.amazon.com/vpc/latest/userguide/default-vpc.html
[link-2]: https://go.dev/
[link-3]: https://www.pulumi.com/
[link-4]: https://github.com/scottslowe/learning-tools/
[link-5]: https://twitter.com/scott_lowe
[link-6]: https://slack.pulumi.com/
