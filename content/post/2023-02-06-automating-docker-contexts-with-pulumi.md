---
author: slowe
categories: Tutorial
comments: true
date: 2023-02-06T09:00:00-07:00
tags:
- AWS
- Docker
- Go
- macOS
- Pulumi
title: Automating Docker Contexts with Pulumi
url: /2023/02/06/automating-docker-contexts-with-pulumi/
---

Since I switched my primary workstation to an M1-based MacBook Pro (see [my review here][xref-1]), I've starting using temporary AWS EC2 instances for compiling code, building [Docker][link-1] images, etc., instead of using laptop-local VMs. I had an older Mac Pro (running [Fedora][link-3]) here in my home office that formerly filled that role, but I've since given that to my son (he's a young developer who wanted a development workstation). Besides, using EC2 instances has the benefit of access when I'm away from my home office. I use [Pulumi][link-2] to manage these instances, and I extended my Pulumi code to also include managing local Docker contexts for me as well. In this post, I'll share the solution I'm using.<!--more-->

For those that aren't already aware, Docker supports SSH-based contexts, which allow you to use the `docker` CLI over an SSH connection to a remote Docker daemon (including one behind an SSH bastion host). This is the functionality I'm using to do remote Docker image builds on an EC2 instance. I wrote a bit about SSH-based Docker contexts [here][xref-2].

When I run `pulumi up` to create the infrastructure, the Pulumi code (written in Go) does a few things:

1. It looks up infrastructure information (VPC, subnets, etc.) from a separate "base stack" that I use to manage all that. This is done via something known as a _StackReference_, which is probably something I should write about soon.
2. It creates a security group to allow the necessary traffic.
3. It launches an EC2 instance in a subnet from the base stack.
4. It creates a new Docker context using the details for the newly-created EC2 instance.

As one might naturally expect, running `pulumi destroy` deletes the security group, terminates the EC2 instance, and removes the Docker context.

For the purposes of this post, I'm only going to focus on step 4. After creating the EC2 instance, I used this snippet of code:

```go
// Use the Command provider to create or destroy the associated Docker context
_, err = local.NewCommand(ctx, "docker-context", &local.CommandArgs{
	Create: pulumi.Sprintf("docker context create --docker host=ssh://%s --description \"SSH-based Docker context to EC2 instance\" %s", dockerInstance.PrivateIp, regionNames[awsRegion]+"-docker"),
	Delete: pulumi.Sprintf("docker context rm %s", regionNames[awsRegion]+"-docker"),
	Update: pulumi.Sprintf("docker context update --docker host=ssh://%s --description \"SSH-based Docker context to EC2 instance\" %s", dockerInstance.PrivateIp, regionNames[awsRegion]+"-docker"),
})
if err != nil {
	log.Printf("error creating local Docker context: %s", err.Error())
}
```

This is using Pulumi's Command provider, which has the ability to execute different local commands based on whether Pulumi is creating resources, updating resources, or deleting resources (i.e, the Command provider knows if you are running `pulumi up` versus `pulumi destroy` and reacts accordingly). Using `pulumi.Sprintf`, which is a Pulumi-specific version of the "normal" `fmt.Sprintf` function that knows how to deal with Pulumi Outputs (see [here][link-4]), it then creates an appropriate `docker context` command to either create, update, or delete an SSH-based Docker context.

As a result, after the `pulumi up` command finishes, I can run `docker context ls` and see a new Docker context for that Docker host that Pulumi just created. After I run `pulumi destroy` to tear down the Docker host `docker context ls` will no longer list the associated Docker context. Handy!

There's only one problem I haven't solved yet---if the Docker context is the active context when I run `pulumi destroy`, then the command to delete the context fails. That, in turn, causes Pulumi to report the `pulumi destroy` operation failed. I will have to manually select a different context, then run `pulumi destroy` again. I'm sure there is a workaround, but I haven't found it yet (to be fair, I haven't spent a great deal of time trying to find the workaround, either).

I'm sure you can find all sorts of novel use cases for Pulumi's Command provider---try it out and let me know what you create! You can contact me [on Twitter][link-5], [on Mastodon][link-6], or in [the Pulumi community Slack][link-7].

[link-1]: https://www.docker.com/
[link-2]: https://www.pulumi.com/
[link-3]: https://getfedora.org
[link-4]: https://www.pulumi.com/docs/intro/concepts/inputs-outputs/
[link-5]: https://twitter.com/scott_lowe
[link-6]: https://fosstodon.org/@scottslowe
[link-7]: https://slack.pulumi.com
[xref-1]: {{< relref "2021-06-02-review-2020-m1-based-macbook-pro.md" >}}
[xref-2]: {{< relref "2019-08-01-accessing-docker-daemon-via-ssh-bastion-host.md" >}}
