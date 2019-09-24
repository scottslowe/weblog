---
author: slowe
categories: Explanation
comments: true
date: 2018-05-23T23:00:00Z
tags:
- CLI
- JSON
- AWS
title: "Quick Post: Parsing AWS Instance Data with JQ"
url: /2018/05/23/quick-post-parsing-aws-instance-data-with-jq/
---

I recently had a need to get a specific subset of information about some AWS instances. Naturally, I turned to the CLI and some CLI tools to help. In this post, I'll share the command I used to parse the AWS instance data down using [the ever-so-handy `jq` tool][link-1].<!--more-->

What I needed, specifically, was the public IP address and the private IP address for each instance. That information is readily accessible using the `aws ec2 describe-instances` command, but that command provides a ton more information than I needed. So, I decided to try to use `jq` to parse the JSON output from the AWS CLI. If you're not familiar with `jq`, I recommend you take a look at [this brief introductory post][xref-1] I wrote back in 2015.

After some trial and error, here's the final command I used:

```shell
aws ec2 describe-instances | jq '.Reservations[] | .Instances[] | \
{Id: .InstanceId, PublicAddress: .PublicIpAddress, \
PrivateAddress: .PrivateIpAddress}'
```

I'll refer you to [the `jq` manual][link-2] for details on breaking down how this filter works. I'll also point out that there's nothing terribly groundbreaking or revolutionary about this command; I wanted to share it here just in case it may save someone a bit of time if they find themselves in a similar situation.

[link-1]: https://stedolan.github.io/jq/
[link-2]: https://stedolan.github.io/jq/manual/v1.5/
[xref-1]: {{< relref "2015-11-11-handy-cli-tool-json.md" >}}
