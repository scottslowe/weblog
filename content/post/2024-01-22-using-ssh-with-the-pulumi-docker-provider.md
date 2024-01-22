---
author: slowe
categories: Explanation
comments: true
date: 2024-01-22T08:30:00-06:00
tags:
- Docker
- Go
- Pulumi
- SSH
title: Using SSH with the Pulumi Docker Provider
url: /2024/01/22/using-ssh-with-the-pulumi-docker-provider/
---

In August 2023, Pulumi released a version of the Docker provider that supported SSH-based connections to a Docker daemon. I've written about using SSH with Docker before (see [here][xref-1]), and I sometimes use AWS-based "Docker build hosts" with my M-series Macs to make it easier/simpler (and sometimes faster) to build x86_64-based Docker images. Naturally, I'm using an SSH connection in those cases. Until this past weekend, however, I hadn't really made the time to look deeper into how to use SSH with the Pulumi Docker provider. In this post, I'll share some details that (unfortunately) haven't yet made it into the documentation about using SSH with the Pulumi Docker provider.<!--more-->

First, let's talk about some prerequisites to making this work.

1. You'll need [Docker][link-2] installed locally. I fairly certain this is _only_ the `docker` CLI (much in the same way the Pulumi Kubernetes provider requires `kubectl` to be installed locally), but I haven't verified this for certain yet. I tested this from a Linux system running Docker 24.0.7; I think the earliest version that is supported is 18.09.
1. You'll need Docker installed on the remote SSH host (obviously). I used [Flatcar Container Linux][link-5] (stable channel) on [AWS][link-4].
1. You'll need [Pulumi][link-1] installed locally. I tested with a pretty recent version of the `pulumi` CLI (v3.101.1).
1. I tested this with the latest version of the Docker provider as of this writing (v4.5.1), using [Go][link-3] 1.21 as the programming language.

You may already be aware that there are a couple of ways to use Pulumi providers when writing Pulumi infrastructure as code programs:

* There's the _default_ provider. The default provider uses what I would call "ambient" configuration---for example, the default AWS provider uses whatever AWS credentials/profile are available (or are specified in the stack configuration), and the default Docker provider uses whatever is specified by the `DOCKER_HOST` environment variable.
* There's also _explicit_ providers. Explicit providers are declared programmatically in your Pulumi program, and you can pass configuration details to the provider when it's declared. You could, for example, declare a couple of explicit AWS providers so that you could provision resources in different accounts or in different regions (from within the same program).

More details on providers can be found [here][link-6].

With regard to the Pulumi Docker provider, this means the following:

* If you want to use the Docker provider against a Docker daemon that is preexisting, then you can use the default provider and supply configuration _either_ through the `DOCKER_HOST` environment variable _or_ via stack configuration (`pulumi config set docker:host <ssh-url>`). (Note that, as of the time of this writing, the Docker provider does not support Docker contexts.)
* If you want to use the Docker provider with a resource being provisioned in the same stack or if you---for whatever reason---need to programmatically assign the Docker daemon endpoint in your program, then you need to use an explicit provider, and configure that explicit provider to use SSH.

Using and configuring the default provider is reasonably straightforward, so in this article I'll focus on the explicit provider; specifically, on the use of an explicit provider to make SSH-based connections to a Docker daemon.

Declaring a basic explicit provider is not terribly complex:

```go
remoteDocker, err := docker.NewProvider(ctx, "remote-docker", &docker.ProviderArgs{})
```

To make the explicit provider actually work in this use case (i.e., connect over SSH to a remote Docker daemon), the configuration is a bit more complex:

```go
remoteDocker, err := docker.NewProvider(ctx, "remote-docker", &docker.ProviderArgs{
    Host: pulumi.Sprintf("ssh://<username>@%s", <ip-address>),
    SshOpts: pulumi.StringArray{
        pulumi.String("-i"), pulumi.String("/path/to/private/key"),
        pulumi.String("-o"), pulumi.String("StrictHostKeyChecking=no"),
        pulumi.String("-o"), pulumi.String("UserKnownHostsFile=/dev/null"),
    },
})
```

You'd need to substitute appropriate values for `username` (on Flatcar you'd likely use "core"), `ip-address`, and `/path/to/private/key`. Since I'm discussing using the Docker provider with a resource provisioned in the same stack, `ip-address` is most likely going to be a reference to the public IP address of an EC2 instance---such as `flatcarInstance.publicIp`. That's also why the code above uses `pulumi.Sprintf`, which is capable of dealing with Outputs in Pulumi code.

The syntax of the `SshOpts` section isn't currently defined in the docs; fortunately, I found a clue [here][link-7] that led to the Go code you see above. Given that this is using a resource that was provisioned in the same stack, the only way to make it work is to disable strict host key checking.

There's one final complication. EC2 instances---or their equivalents on Azure or Google Cloud---take a small amount of time to boot up and become ready. The Docker provider needs to check the connection, and if it attempts that before the remote host is ready it will throw an error.

"No problem!" you say. "Just throw a `sleep` in there."

Well...Pulumi doesn't necessarily execute your Go code in the way you might normally expect, so this won't work. What we need to do is create a _resource_ that the Pulumi engine can add to the dependency graph that will insert a delay before creating the Docker provider. Fortunately, there is [a Time provider][link-8] that provides [a Sleep resource][link-9] to accomplish exactly what we need. To create the necessary dependencies and insert the delay in the right place, we make the Sleep resource dependent on the EC2 instance and the Docker provider dependent on the Sleep resource.

Including the EC2 instance, the Sleep resource, and the Docker provider, the code now looks like this (I've omitted error checking code for simplicity):

```go
// Launch an instance using Flatcar Linux AMI
flatcarInstance, err := ec2.NewInstance(ctx, "flatcar-instance", &ec2.InstanceArgs{
    Ami:                      pulumi.String(flatcarAmi.Id),
    InstanceType:             pulumi.String(instanceType),
    AssociatePublicIpAddress: pulumi.Bool(true),
    KeyName:                  pulumi.StringPtr(userSuppliedKeyPair),
    SubnetId:                 dockerVpc.PublicSubnetIds.Index(pulumi.Int(0)),
    VpcSecurityGroupIds:      pulumi.StringArray{dockerSg.ID()},
    Tags: pulumi.StringMap{
        "Name": pulumi.String("flatcar-instance"),
    },
})

// Sleep for 20 seconds to allow instance to boot
instanceBootDelay, err := time.NewSleep(ctx, "instance-boot-delay", &time.SleepArgs{
    CreateDuration: pulumi.String("20s"),
}, pulumi.DependsOn([]pulumi.Resource{flatcarInstance}))

// Create a new Docker provider
remoteDocker, err := docker.NewProvider(ctx, "remote-docker", &docker.ProviderArgs{
    Host: pulumi.Sprintf("ssh://core@%s", flatcarInstance.PublicIp),
    SshOpts: pulumi.StringArray{
        pulumi.String("-i"), pulumi.String(userSuppliedPrivateKeyFile),
        pulumi.String("-o"), pulumi.String("StrictHostKeyChecking=no"),
        pulumi.String("-o"), pulumi.String("UserKnownHostsFile=/dev/null"),
    },
}, pulumi.DependsOn([]pulumi.Resource{instanceBootDelay}))
```

Following this code, you could then have the remote Docker daemon pull an image and deploy a container from that image:

```go
// Pull down a container image on the remote host
nginxImage, err := docker.NewRemoteImage(ctx, "nginx-image", &docker.RemoteImageArgs{
    Name: pulumi.String("nginx:1.17.4-alpine"),
}, pulumi.Provider(remoteDocker))

// Launch a container on the remote host
_, err = docker.NewContainer(ctx, "nginx-container", &docker.ContainerArgs{
    Image: nginxImage.ImageId,
}, pulumi.Provider(remoteDocker))
```

Neat, right? (And useful!) In one Pulumi stack, you can provision the EC2 instance, create a Docker provider to communicate with that instance, and deploy containers on that instance---all in the programming language of your choice.

If you'd like to see the full Pulumi program, you can find it in the `docker/docker-ssh-pulumi` folder of [my GitHub "learning-tools" repository][link-10]. The code is useful in that it illustrates the correct syntax for a number of useful constructs when using Pulumi with Go:

* Creating a dependency between resources
* Referencing an explicit provider for a resource
* Configuring the SSH options for the Docker provider
* Looking up the AMI for an instance (not shown above, but it is in the full code on GitHub)

I hope this proves useful to someone. If you have questions, you are welcome to open an issue on [my "learning-tools" GitHub repository][link-10], or you can reach out to me directly. You can contact [me on Twitter][link-11], [on the Fediverse][link-12], or via Slack (I'm active in a number of different Slack communities). I'd love to hear from you, hear any feedback you might have, or try to answer questions about this article or any of my articles. Thanks for reading!

[link-1]: https://www.pulumi.com/
[link-2]: https://www.docker.com/
[link-3]: https://go.dev/
[link-4]: https://aws.amazon.com/
[link-5]: https://flatcar.org/
[link-6]: https://www.pulumi.com/docs/concepts/resources/providers/
[link-7]: https://github.com/pulumi/pulumi-docker/blob/014b3fa8b3d9369d4108e71006cf8d429c19bc13/examples/test-ssh-conn/ts/index.ts#L26-L33
[link-8]: https://www.pulumi.com/registry/packages/time/
[link-9]: https://www.pulumi.com/registry/packages/time/api-docs/sleep/
[link-10]: https://github.com/scottslowe/learning-tools/
[link-11]: https://twitter.com/scott_lowe
[link-12]: https://fosstodon.org/@scottslowe
[xref-1]: {{< relref "2019-08-01-accessing-docker-daemon-via-ssh-bastion-host.md" >}}
