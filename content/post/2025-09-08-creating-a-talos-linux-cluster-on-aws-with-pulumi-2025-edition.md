---
author: slowe
categories: Tutorial
comments: true
date: 2025-09-08T09:35:00-04:00
tags:
- Automation
- AWS
- Go
- Kubernetes
- Pulumi
- Talos
title: "Creating a Talos Linux Cluster on AWS with Pulumi, 2025 Edition"
url: /2025/09/08/creating-a-talos-linux-cluster-on-aws-with-pulumi-2025-edition/
---

A little over two years ago, I wrote a post on [creating a Talos Linux cluster on AWS using Pulumi][xref-1]. At the time of that post, the Pulumi provider for Talos was still a prerelease version. Since then, the Talos provider has undergone some notable changes necessitating an update to the example code I have on GitHub. For your reading pleasure, therefore, I present you with the 2025 edition of a tutorial for using Pulumi to create a Talos Linux cluster on AWS.<!--more-->

The updated [Pulumi][link-1] code can be found in [this GitHub repository][link-2]. Note that I've tagged the original version from the 2023 blog post with the "2023-post" tag, in the event you'd like to see the original code. While I chose to write my Pulumi code in [Go][link-5], note that Pulumi supports a number of different languages (such as JavaScript/TypeScript, Python, one of the .NET languages, Java, or even YAML). I leave it as an exercise for the reader to re-implement this functionality in a different language. This Pulumi program is based on [the Talos documentation for standing up a cluster on AWS][link-3].

The Pulumi program has four major sections:

1. First, it creates the underlying base infrastructure needed for a Talos Linux cluster to run. This includes a VPC (and all the assorted other pieces, like subnets, gateways, routes, and route tables) and a load balancer. The load balancer is needed for the Kubernetes control plane, which we will bootstrap later in the program. This portion also creates the EC2 instances for the control plane.
2. Next, it uses the Talos Pulumi provider to generate the Talos configuration that will be applied to the instances after they are created. This configuration needs information from step 1; specifically, it needs the DNS name of the load balancer and the IP addresses of the control plane nodes.
3. Third, it applies the correct configuration to the control plane nodes, and launches some EC2 instances to serve as worker nodes in the Kubernetes cluster. The program then takes the Talos configuration from step 2 and applies it to the worker nodes.
4. The fourth and final step is to bootstrap the cluster.

For those who need a "TL;DR": you can clone [the associated repository][link-2], run `pulumi up`, and have a working Talos Linux cluster in a few minutes.

For those who want a bit more detail, read on.

## Creating the AWS Infrastructure

The first part of the Pulumi program (running through about line 270) creates the necessary AWS infrastructure. The program leverages [Crosswalk for AWS][link-4] to simplify the creation of the underlying VPC and associated components. Most of the code involves creating security groups and security group rules.

This part of the program also creates the load balancer that will be used for the Kubernetes API.

Finally, the program launches three EC2 instances using one of the official Talos AMIs. In the original version of the code, you had to supply the AMI ID as a configuration value; this version looks up the AMI when it runs. The IDs and the private IP addresses for these EC2 instances are stored in two different arrays; we'll need this information later.

## Creating the Talos Configuration

The next part of the Pulumi program builds the Talos configuration, using the  Pulumi provider and information (outputs) from the resources created earlier. Specifically, the Talos configuration uses the DNS name of the load balancer created earlier, and the IP addresses of the EC2 instances (which will become the control plane nodes for the Kubernetes cluster).

There are three parts to the configuration:

1. A client configuration file, which the `talosctl` command-line utility uses to connect to the Talos Linux nodes
2. Machine configuration for the control plane nodes
3. Machine configuration for the worker nodes

## Configuring the Nodes

Once the Talos provider builds the correct machine configurations, the program then applies the configuration to each of the control plane nodes (worker nodes are handled later). The most efficient way would be to `range` over the array with the instance IP addresses and apply the configuration to each instance. However, in this particular case we need to build a dependency on applying the machine configuration, so the program does it "manually."

Once the machine configuration has been applied to the control plane nodes, three more EC2 instances are launched for worker nodes, and then the correct machine configuration is applied to those nodes (this time iterating through the array).

## Bootstrapping the Cluster

Finally, it comes down to bootstrapping the cluster (see line 397), which---again using the Talos provider for Pulumi---is accomplished with calling `machine.NewBootstrap` against the first control plane node to be created. The Pulumi program will complete after this; it doesn't wait for the bootstrap process to complete (which will take a few more minutes).

At this point, you can use `pulumi stack output` to retrieve a configuration file for the `talosctl` command-line utility, and then run `talosctl health` to watch the cluster bootstrap. Once the cluster is up and running, `talosctl kubeconfig` will get you a Kubeconfig file you can use to access the Kubernetes API via `kubectl`. Nice!

## Additional Resources

The updated Pulumi code is available on GitHub in [this repository][link-2]. Please note that I tested the code and it works for me, but it is supplied "as-is". Don't use this for your production environment without performing your own testing and validation!

Thanks for reading! I hope this post is helpful. If you have questions, you're welcome to open an issue in [the GitHub repository][link-2], or you can contact me directly. If you have a potential enhancement to the code, you're invited to open a pull request. For more in-depth discussions, it's not hard to get in touch with me---you can find me [on Twitter][link-96], [on Mastodon][link-97], and on various Slack communities (including the [Kubernetes][link-98] and [Pulumi][link-99] Slack communities).

[link-1]: https://www.pulumi.com/
[link-2]: https://github.com/scottslowe/talos-aws-pulumi/
[link-3]: https://www.talos.dev/v1.11/talos-guides/install/cloud-platforms/aws/
[link-4]: https://www.pulumi.com/docs/iac/clouds/aws/guides/
[link-5]: https://go.dev
[link-96]: https://twitter.com/scott_lowe
[link-97]: https://fosstodon.org/@scottslowe
[link-98]: https://kubernetes.slack.com
[link-99]: https://slack.pulumi.com
[xref-1]: {{< relref "2023-02-24-creating-a-talos-linux-cluster-on-aws-with-pulumi.md" >}}
