---
author: slowe
categories: Explanation
comments: true
date: 2024-01-03T11:00:00-06:00
tags:
- CLI
- IaC
- JSON
- Pulumi
title: Selectively Replacing Resources with Pulumi
url: /2024/01/03/selectively-replacing-resources-with-pulumi/
---

Because [Pulumi][link-1] operates declaratively, you can write a Pulumi program that you can safely run (via `pulumi up`) multiple times. If no changes are needed---meaning that the current state of the infrastructure matches what you've defined in your Pulumi program---then nothing happens. If only one resource needs to be updated, then it will update only that one resource (and any dependencies, if there are any). There may be times, however, when you want to _force_ the replacement of specific resources. In this post, I'll show you how to target specific resources for replacement when using Pulumi.<!--more-->

Here's an example: I use Pulumi to manage my [AWS][link-2]-based lab resources, including [SSH bastion hosts][xref-1]. However, because my code uses a dynamic AMI lookup, I've instructed Pulumi to ignore changes in the AMI ID for the bastion hosts (by appending `pulumi.IgnoreChanges([]string{"ami"})` as a resource option). This gives me the control over when the bastion hosts get replaced, instead of Pulumi wanting to replace them every time the AMI ID changes.

With this in place, then, how do I tell Pulumi that I'm ready to replace the bastion hosts? Tearing down the entire stack isn't an option. Fortunately, the `pulumi` CLI has a command-line flag that enables users to selectively destroy resources. The command looks like this:

```shell
pulumi destroy --target <urn>
```

This will destroy _only_ the specified URNs (you can specify multiple `--target <urn>` flags). On the next run of `pulumi up`, Pulumi will note that the resources are missing, and recreate them---in this case, using the latest AMI ID.

So how does one get the URNs for the resources? The `pulumi stack export` command outputs JSON, and you can use tools like `gron` and `jq` to find and parse out the specific URNs you need. For example, to find EC2 instances in the stack, you could start like this:

```shell
pulumi stack export | gron - | grep 'ec2/instance:Instance'
```

This will return some "flattened" JSON that will provide the specific path in the JSON output for any EC2 instances. You could then combine that with `jq` to parse/extract the URN, like this:

```shell
pulumi stack export | jq '.deployment.resources[21].urn'
```

(Obviously, you'd need to change the `resources[21]` to match whatever resources were returned by the earlier command with `gron`.)

Once you're armed with the appropriate URNs, you can pass them to the `pulumi destroy --target <urn>` command to destroy the specific resources.

I will say that this all seems pretty obvious in retrospect, but I hope this information is useful to someone nevertheless. If you have any questions, feel free to find me online and I'll do my best to answer them. I'm active [on Twitter][link-3], on [the Fediverse][link-4], and in various Slack communities (especially [the Pulumi Community Slack][link-5]).

[link-1]: https://www.pulumi.com/
[link-2]: https://aws.amazon.com/
[link-3]: https://twitter.com/scott_lowe
[link-4]: https://fosstodon.org/@scottslowe
[link-5]: https://slack.pulumi.com
[xref-1]: {{< relref "2015-11-21-using-ssh-bastion-host.md" >}}
