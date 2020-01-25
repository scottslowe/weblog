---
author: slowe
categories: Tutorial
comments: true
date: 2020-01-25T10:32:00-07:00
tags:
- Pulumi
- TypeScript
- AWS
- Automation
title: Creating an AWS VPC Endpoint with Pulumi
url: /2020/01/25/creating-aws-vpc-endpoint-with-pulumi/
---

In this post, I'd like to show readers how to use [Pulumi][link-2] to create a VPC endpoint on AWS. Until recently, I'd heard of VPC endpoints but hadn't really taken the time to fully understand what they were or how they might be used. That changed when I was presented with a requirement for the AWS EC2 APIs to be available within a VPC that did not have Internet access. As it turns out---and as many readers are probably already aware---this is one of the key use cases for a VPC endpoint (see [the VPC endpoint docs][link-3]). The sample code I'll share below shows how to programmatically create a VPC endpoint for use in infrastructure-as-code use cases.<!--more-->

For those that aren't familiar, Pulumi allows users to use one of a number of different general-purpose programming languages and apply them to infrastructure-as-code scenarios. In this example, I'll be using [TypeScript][link-4], but Pulumi also supports JavaScript and Python (and [Go][link-5] is in the works). _(Side note: I intend to start working with the Go support in Pulumi when it becomes generally available as a means of helping accelerate my own Go learning.)_

Here's a snippet of TypeScript code that will create a VPC endpoint (I've randomized the resource IDs of the AWS resources below):

```typescript
const ec2ApiEndpoint = new aws.ec2.VpcEndpoint("test-vpc-endpoint", {
    privateDnsEnabled: true,
    serviceName: "com.amazonaws.us-west-2.ec2",
    securityGroupIds: [ "sg-1db01e1f9be081d76" ],
    subnetIds: [ "subnet-0db757befe2c48624" ],
    vpcEndpointType: "Interface",
    vpcId: "vpc-0e2c348614e845763",
    tags: {
        Name: "test-vpc-endpoint"
    }
});
```

Let's walk through this a bit (the full API reference is [here][link-6]):

* The `privateDnsEnabled: true` line enables us to use the "standard" EC2 API URL endpoint within the VPC and have it resolve to a private IP address out of the VPC/subnets. This relies on Route 53, and is necessary for use cases where you (the user) don't have any control over how the API endpoint is being called from within the VPC (it may be a bit of commercial software you can't modify). In this case, any software trying to reach "ec2.us-west-2.amazonaws.com" will resolve to a private IP address instead of the public IP address.
* The `serviceName` specifies which service is available via this endpoint (naturally, this is for EC2).
* The `vpcEndpointType` specifies "Interface", which means that an ENI is created to allow traffic to reach the service represented by this endpoint. The other option is "Gateway", which would require you to modify route tables, and is only supported for some services (like S3). [This article][link-1] provides an example of a gateway endpoint.

The other settings are all pretty self-explanatory. This being TypeScript, you could---of course---use references to variables or outputs from other portions of your Pulumi code instead of hard-coding values as I have in this example (and that would be highly recommended). This could be easily incorporated into a large portion of code that created a new VPC, new subnets, etc.

And that's it! I know this is a fairly simple example, but I hadn't found very many articles showing how this is done, and thought that sharing this may prove helpful to others. Feel free to [contact me on Twitter][link-7] if you have corrections (in the event I made a mistake), suggestions for improvement, or other feedback.

[link-1]: https://cloudaffaire.com/create-a-vpc-interface-endpoint/
[link-2]: https://www.pulumi.com/
[link-3]: https://docs.aws.amazon.com/vpc/latest/userguide/vpc-endpoints.html
[link-4]: http://www.typescriptlang.org/
[link-5]: https://golang.org/
[link-6]: https://www.pulumi.com/docs/reference/pkg/nodejs/pulumi/aws/ec2/#VpcEndpoint
[link-7]: https://twitter.com/scott_lowe
