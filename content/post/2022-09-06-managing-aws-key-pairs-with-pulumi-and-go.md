---
author: slowe
categories: Explanation
comments: true
date: 2022-09-06T10:50:00-06:00
tags:
- Automation
- AWS
- Go
- IaC
- Pulumi
- SSH
title: Managing AWS Key Pairs with Pulumi and Go
url: /2022/09/06/managing-aws-key-pairs-with-pulumi-and-go/
---

As I was winding down things at [Kong][link-2] and getting ready to transition to [Pulumi][link-3] (more information on why I moved to Pulumi [here][xref-1]), I casually made [the comment on Twitter][link-4] that I needed to start managing my AWS key pairs using Pulumi. When the opportunity arose last week, I started doing exactly that! In this post, I'll show you a quick example of how to use Pulumi and [Go][link-5] to declaratively manage AWS key pairs.<!--more-->

This is a pretty simple example, so let's just jump straight to the code:

```go
_, err := ec2.NewKeyPair(ctx, "aws-rsa-keypair", &ec2.KeyPairArgs{
    KeyName:   pulumi.String("key-pair-name"),
    PublicKey: pulumi.String("<ssh-key-material-here>"),
    Tags: pulumi.StringMap{
        "Owner":   pulumi.String("User Name"),
        "Team":    pulumi.String("Team Name"),
        "Purpose": pulumi.String("Public key for authenticating to AWS EC2 instances"),
        },
    })
    if err != nil {
        return err
    }
```

This code is, by and large, pretty self-explanatory. For `PublicKey`, you just need to supply the contents of the public key file (use `cat` or similar to get the contents of the file) where it shows `<ssh-key-material-here>`. Then specify an appropriate name and adjust the tags as desired. In this particular case, I'm "throwing away" the reference to the newly-created key pair (note the `_, err := ec2.NewKeyPair`; the underscore indicates I'm discarding that value) because I don't need to refer to it anywhere else. If I _did_ need to refer to it elsewhere (say, I was going to create a new key pair and launch an instance in the same Pulumi program), then I'd want to catch that return value.

For more information, the API docs for this resource are found [here][link-1].

In the event you're wondering how this snippet of code manages key pairs in multiple regions...well, that's where _stacks_ come into play. You can create multiple stacks for any Pulumi project (including this one), and each stack can target a specific AWS region. This is exactly the approach I'm using. For completeness' sake, the process looks something like this:

1. Run `pulumi stack init` to create a new stack. Specify the name of the new stack.
2. Run `pulumi config set aws:region <asw-region>`, replacing `<aws-region>` with the name of the AWS region this stack should target.
3. Run `pulumi up` and you're good to go.

I hope this example is useful to someone. If you have any questions, feel free to reach out to [me on Twitter][link-6] or find me in [the Pulumi community Slack][link-7] (sign up for the Pulumi community Slack [here][link-8] if you don't already have an account).

[link-1]: https://www.pulumi.com/registry/packages/aws/api-docs/ec2/keypair/
[link-2]: https://konghq.com/
[link-3]: https://www.pulumi.com/
[link-4]: https://twitter.com/scott_lowe/status/1559928143972032512
[link-5]: https://go.dev/
[link-6]: https://twitter.com/scott_lowe
[link-7]: https://pulumi-community.slack.com/
[link-8]: https://slack.pulumi.com/
[xref-1]: {{< relref "2022-08-15-jumping-off-cliffs.md" >}}
