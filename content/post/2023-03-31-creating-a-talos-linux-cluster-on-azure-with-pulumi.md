---
author: slowe
categories: Tutorial
comments: true
date: 2023-03-31T08:30:00-06:00
tags:
- Automation
- Azure
- Go
- Kubernetes
- Pulumi
- Talos
title: Creating a Talos Linux Cluster on Azure with Pulumi
url: /2023/03/31/creating-a-talos-linux-cluster-on-azure-with-pulumi/
---

A little over a month ago I published a post on [creating a Talos Linux cluster on AWS with Pulumi][xref-1]. [Talos Linux][link-1] is a re-thinking of your typical Linux distribution, custom-built for running [Kubernetes][link-3]. Talos Linux has no SSH access, no shell, and no console; instead, everything is managed via a gRPC API. This post is something of a "companion post" to the earlier AWS post; in this post, I'll show you how to create a Talos Linux cluster on [Azure][link-8] with [Pulumi][link-2].<!--more-->

The program I'll share with you in this post is written in [Go][link-4], but the process outlined in this post and the accompanying code is equally applicable in other languages supported by Pulumi. (TypeScript is a popular choice for lots of folks.) The code is available in [this GitHub repository][link-6]. It's based on [this documentation from Sidero Labs][link-5], and I also found [this blog post][link-7] to be helpful as well.

The Pulumi program follows this overall flow:

1. First, the program creates the base infrastructure objects that are required---a resource group, a virtual network, some subnets, and a network security group.
2. Next, it creates a load balancer, gets a public IP address for the load balancer, and creates the associated backend address pool, health probe, and load balancing rule. (This load balancer is used only for Kubernetes API traffic.)
3. The program uses the Talos provider to generate configurations for the control plane VMs and the worker VMs.
4. Next up are the public IP addresses, network interfaces, and the control plane VMs (along with the necessary associations to put the interfaces into the backend address pool for the load balancer and the network security group). The Talos machine configuration is passed to the VMs at startup.
5. Last, but certainly not least, the program creates the worker VMs and their network interfaces, along with the association required to make the interfaces part of the network security group. As with the control plane VMs, the Talos machine configuration is passed to the VMs at startup.
6. The last step is to bootstrap the cluster.

Let's take a bit of a closer look at each of these steps.

## Creating the Azure infrastructure

The first part of the Pulumi program creates the requisite base Azure infrastructure: a resource group to contain all the resources, a virtual network, three subnets, and a network security group to control the traffic allowed to the VMs.

There's one very important task that the Pulumi program does _not_ resolve for the user: the creation of a Talos Linux virtual machine image. You'll want to follow [the documentation][link-5] for the steps to upload a Talos Linux VHD file and register an image, and then use that image's ID with `pulumi config set talosImageId <id-of-image-you-created>` before running `pulumi up`.

## Creating the Kubernetes API Load Balancer

Because the program sets out to create three control plane nodes for an HA control plane, a load balancer to handle the Kubernetes API traffic is needed. On Azure, this means creating a number of different things:

* The load balancer itself
* A public IP address to be assigned to the "front end" of the load balancer
* A backend address pool (where the control plane VMs will join to be directed traffic)
* A health probe
* A load balancing rule that instructs the load balancer on how and where to direct traffic

All of this takes you up through line 184 of the Pulumi Go program.

## Creating the Talos Configuration

Next up is creating the machine configurations that will be applied to the control plane and worker VMs. First, though, the program has to create some resources first: public IP addresses and network interfaces for the control plane VMs, along with associating these interfaces with the backend address pool and the network security group.

Once those addresses and interfaces are created, they're used---along with the public IP address created for the load balancer---to generate the machine configurations for the VMs that will actually run Talos Linux.

There are four parts to the configuration generated:

1. Some PKI assets, referred to here as machine secrets
2. A client configuration file, which the `talosctl` command-line utility uses to connect to the Talos Linux nodes
3. Machine configuration for the control plane nodes
4. Machine configuration for the worker nodes

All of these are generated using the prerelease Talos Linux provider for Pulumi. To install the prerelease provider, you can follow [these instructions][xref-2].

## Launching the Nodes

Once the Talos configurations have been created, then the program moves on to create the control plane VMs. These are linked to the network interfaces (and public IP addresses) used earlier. Since those interfaces were associated with the network security group, the correct traffic is permitted; and since the interfaces were associated with the backend address pool, the load balancer will send Kubernetes API traffic to them.

The worker VMs are handled similarly, except they aren't given public IP addresses, and aren't registered to the load balancer's backend address pool.

In both cases---for both the control plane and the worker VMs---the appropriate machine configuration is passed to the VM at startup.

## Bootstrapping the Cluster

Finally, it comes down to bootstrapping the cluster, which---again using the Talos provider for Pulumi---is accomplished with calling `talos.NewTalosMachineBootstrap` against the first control plane VM to be created. The Pulumi program will complete after this; it doesn't wait for the bootstrap process to complete (which will take a few more minutes).

At this point, you can use `pulumi stack output` to retrieve a configuration file for the `talosctl` command-line utility, and then run `talosctl health` to watch the cluster bootstrap. Once the cluster is up and running, `talosctl kubeconfig` will get you a Kubeconfig file you can use to access the Kubernetes API via `kubectl`. Boom---just like that you've got a Kubernetes cluster.

## Using this Pulumi Program

As mentioned earlier in this post, all of the Pulumi code is available on GitHub in [this repository][link-6]. Please note that I tested the code and it works for me, but it is supplied "as-is". Don't use this for your production environment without performing your own testing and validation!

If you do want to give it a spin, you'll need to (these steps assume you've already installed Pulumi, the Azure CLI, and Go):

1. Create your own Talos Linux image (by uploading a VHD and registering it as an image)
2. Clone the `talos-azure-pulumi` GitHub repository to your machine and switch into that directory.
3. Run `pulumi stack init` to create a new stack.
4. Run `pulumi config set talosImageId <image-id>` to set the ID of the image you created in step 1.
5. Run `pulumi config set azure:location <region-name>` to set the Azure region where you want your resources created.
6. Run `pulumi up`.

That's it!

## Additional Resources

In addition to this blog post, the code and instructions are found in [the associated GitHub repository][link-6]. The code contains links to the API documentation for both the Pulumi SDK and the Talos provider; check the comments in the code.

I hope this is helpful to you! If you have questions, you're welcome to open an issue in [the GitHub repository][link-6], or you can contact me directly. It's not hard to get in touch with me---you can find me [on Twitter][link-96], [on Mastodon][link-97], and on various Slack communities (including the [Kubernetes][link-98] and [Pulumi][link-99] Slack communities).

[link-1]: https://www.talos.dev
[link-2]: https://www.pulumi.com
[link-3]: https://kubernetes.io
[link-4]: https://go.dev
[link-5]: https://www.talos.dev/v1.3/talos-guides/install/cloud-platforms/azure
[link-6]: https://github.com/scottslowe/talos-azure-pulumi
[link-7]: https://blog.nillsf.com/index.php/2021/10/29/setting-up-kubernetes-on-azure-using-kubeadm/
[link-8]: https://azure.microsoft.com/en-us/
[link-96]: https://twitter.com/scott_lowe
[link-97]: https://fosstodon.org/@scottslowe
[link-98]: https://kubernetes.slack.com
[link-99]: https://slack.pulumi.com
[xref-1]: {{< relref "2023-02-24-creating-a-talos-linux-cluster-on-aws-with-pulumi.md" >}}
[xref-2]: {{< relref "2023-02-08-installing-prerelease-pulumi-provider-talos.md" >}}
