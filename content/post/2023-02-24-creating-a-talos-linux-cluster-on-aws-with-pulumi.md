---
author: slowe
categories: Tutorial
comments: true
date: 2023-02-24T13:30:00-07:00
tags:
- Automation
- AWS
- Go
- Kubernetes
- Pulumi
- Talos
title: Creating a Talos Linux Cluster on AWS with Pulumi
url: /2023/02/24/creating-a-talos-linux-cluster-on-aws-with-pulumi/
---

[Talos Linux][link-1] is a Linux distribution purpose-built for running [Kubernetes][link-3]. The Talos web site describes Talos Linux as "secure, immutable, and minimal." All system management is done via an API; there is no SSH access, no shell, and no console. In this post, I'll share how to use [Pulumi][link-2] to automate the creation of a Talos Linux cluster on AWS.<!--more-->

I chose to write my Pulumi program in [Go][link-5], but you could---of course---choose to write it in any language that Pulumi supports (JavaScript/TypeScript, Python, one of the .NET languages, Java, or even YAML). I've made the Pulumi program available via [this GitHub repository][link-6]. It's based on [these instructions][link-4] for standing up Talos Linux on AWS.

The Pulumi program has four major sections:

1. First, it creates the underlying base infrastructure needed for a Talos Linux cluster to run. This includes a VPC (and all the assorted other pieces, like subnets, gateways, routes, and route tables) and a load balancer. The load balancer is needed for the Kubernetes control plane, which we will bootstrap later in the program. This portion also creates the EC2 instances for the control plane.
2. Next, it uses the Talos Pulumi provider to generate the Talos configuration that will be applied to the instances after they are created. This configuration needs information from step 1; specifically, it needs the DNS name of the load balancer and the IP addresses of the control plane nodes.
3. Third, it applies the correct configuration to the control plane nodes, and launches some EC2 instances to serve as worker nodes in the Kubernetes cluster. The program then takes the Talos configuration from step 2 and applies it to the worker nodes.
4. The fourth and final step is to bootstrap the cluster.

Let's examine each of these sections in a bit more detail.

## Creating the AWS Infrastructure

The first part of the Pulumi program (running through about line 197) creates the necessary AWS infrastructure. The program leverages [Crosswalk for AWS][link-7] to simplify the creation of the underlying VPC and associated components. Most of the code involves creating security groups and security group rules.

This part of the program also creates the load balancer that will be used for the Kubernetes API.

Finally, the program launches three EC2 instances using one of the official Talos AMIs (you have to supply the AMI ID as a configuration value). The IDs and the private IP addresses for these EC2 instances are stored in two different arrays; we'll need this information later.

Note that you could extend the Pulumi Go program to look up the correct AMI for you so that this information doesn't need to be passed in as a configuration value via `pulumi config`. I'll leave this as an exercise for the reader.

## Creating the Talos Configuration

The next part of the Pulumi program builds the Talos configuration, using the prerelease Pulumi provider and information (outputs) from the resources created earlier. Specifically, the Talos configuration uses the DNS name of the load balancer created earlier, and the IP addresses of the EC2 instances (which will become the control plane nodes for the Kubernetes cluster).

There are three parts to the configuration:

1. A client configuration file, which the `talosctl` command-line utility uses to connect to the Talos Linux nodes
2. Machine configuration for the control plane nodes
3. Machine configuration for the worker nodes

To install the prerelease provider, you can follow [these instructions][xref-1].

## Configuring the Nodes

Once the Talos provider builds the correct machine configurations, the program then applies the configuration to each of the control plane nodes (worker nodes are handled later). It does this by looping through the array with the instance IP addresses and applying the configuration to each instance.

Once the machine configuration has been applied to the control plane nodes, three more EC2 instances are launched for worker nodes, and then the correct machine configuration is applied to those nodes (using the mechanism of looping over an array).

## Bootstrapping the Cluster

Finally, it comes down to bootstrapping the cluster, which---again using the Talos provider for Pulumi---is accomplished with calling `talos.NewTalosMachineBootstrap` against the first control plane node to be created. The Pulumi program will complete after this; it doesn't wait for the bootstrap process to complete (which will take a few more minutes).

At this point, you can use `pulumi stack output` to retrieve a configuration file for the `talosctl` command-line utility, and then run `talosctl health` to watch the cluster bootstrap. Once the cluster is up and running, `talosctl kubeconfig` will get you a Kubeconfig file you can use to access the Kubernetes API via `kubectl`. Nice!

## Additional Resources

All of the Pulumi code is available on GitHub in [this repository][link-6]. Please note that I tested the code and it works for me, but it is supplied "as-is". Don't use this for your production environment without performing your own testing and validation!

I hope you've found this post helpful. If you have questions, you're welcome to open an issue in [the GitHub repository][link-6], or you can contact me directly. It's not hard to get in touch with me---you can find me [on Twitter][link-96], [on Mastodon][link-97], and on various Slack communities (including the [Kubernetes][link-98] and [Pulumi][link-99] Slack communities).

[link-1]: https://www.talos.dev/
[link-2]: https://www.pulumi.com/
[link-3]: https://kubernetes.io/
[link-4]: https://www.talos.dev/v1.3/talos-guides/install/cloud-platforms/aws/
[link-5]: https://go.dev/
[link-6]: https://github.com/scottslowe/talos-aws-pulumi/
[link-7]: https://www.pulumi.com/docs/guides/crosswalk/aws/
[link-96]: https://twitter.com/scott_lowe
[link-97]: https://fosstodon.org/@scottslowe
[link-98]: https://kubernetes.slack.com
[link-99]: https://slack.pulumi.com
[xref-1]: {{< relref "2023-02-08-installing-prerelease-pulumi-provider-talos.md" >}}
