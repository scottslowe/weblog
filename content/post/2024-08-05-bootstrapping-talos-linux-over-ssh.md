---
author: slowe
categories: Tutorial
comments: true
date: 2024-08-05T11:00:00-06:00
tags:
- AWS
- CLI
- Linux
- SSH
- Talos
title: Bootstrapping Talos Linux over SSH
url: /2024/08/05/bootstrapping-talos-linux-over-ssh/
---

For those that aren't aware, Talos Linux is a purpose-built Linux distribution designed for running Kubernetes. Bootstrapping a Talos Linux cluster is normally done via the Talos API, but this requires direct network access to the Talos Linux nodes. What happens if you don't have direct network access to the nodes? In this post, I'll share with you how to bootstrap a Talos Linux cluster over SSH.<!--more-->

In all honesty, if you can establish direct network access to the [Talos Linux][link-3] nodes then that's the recommended approach. Consider what I'm going to share here as a workaround---a backup plan, if you will---in the event you can't establish direct network access. I figured out how to bootstrap a Talos Linux cluster over SSH only because I was _not_ able to establish direct network access.

Before getting into the details, I think it's useful to point out that I'm talking about using SSH port forwarding (SSH tunneling) here, but Talos Linux doesn't support SSH (as in, you can't SSH into a Talos Linux system). In this case, you could do the same thing I did and use [an SSH bastion host][xref-3] to handle the port forwarding.

There are two parts to bootstrapping a Talos Linux cluster over SSH:

1. Modifying the machine configurations
2. Using SSH port forwarding

Let's look at each of these.

## Modifying the Machine Configurations

Talos Linux uses a structured machine configuration (reference details [here][link-4]) to control how nodes are configured. Part of this configuration determines the certificates that are provisioned on the node for cryptographic authentication against the Talos API endpoint (which normally listens on TCP port 50000). Because you're using SSH port forwarding to gain access to the Talos API, you need to add a Subject Alternative Name (SAN) to the certificate via the machine configuration.

To accomplish that, I chose to use a patch file, written in YAML, that gets merged into the machine configuration:

```yaml
machine:
  certSANs:
    - 127.0.0.1
```

This patch is used with `talosctl gen config`:

```bash
talosctl gen config <cluster-name> <kubernetes-api-endpoint> --config-patch @patch.yaml
```

Without this patch in place, the certificate would lack the "127.0.0.1" SAN, which means attempts to connect to the Talos node via an SSH tunnel---which redirects a local port to a remote port---would fail. Adding the SAN enables you to use SSH tunnels to access the Talos API endpoint.

Once your machine configurations have been updated to include this additional SAN, you're ready to actually use SSH tunnels to bootstrap the cluster.

## Using SSH Port Forwarding

You'll use SSH port forwarding (aka SSH tunnels) in two ways to bootstrap the cluster:

1. To apply the machine configurations
2. To actually initiate the cluster bootstrap process, to watch the process, and to retrieve the admin Kubeconfig

The first of these two ways involves the use of the `talosctl apply-config` command, which _does_ allow the user to specify a target port number in the command. This means you can establish multiple tunnels (forward multiple ports) in a single command, like this:

```bash
ssh -L 50000:<first-node-ip-address>:50000 -L 50020:<second-node-ip-address>:50000 -L 50030:<third-node-ip-address>:50000 -N -f <ssh-bastion-host>
```

With the port forwarding rules in place, then apply the machine configuration:

```bash
talosctl apply-config -f <patched-config.yaml> -n 127.0.0.1:50000 --insecure
talosctl apply-config -f <patched-config.yaml> -n 127.0.0.1:50020 --insecure
talosctl apply-config -f <patched-config.yaml> -n 127.0.0.1:50030 --insecure
```

Generally you'd start with the control plane nodes first.

Once the machine configuration is in place, then you'll need to use the `talosctl bootstrap` command. This command does _NOT_ allow you to specify a target port---it always assumes port 50000. (Hence the use of port 50000 in the SSH port forwarding command above.)

Start with kicking off the bootstrap process:

```bash
talosctl bootstrap -e 127.0.0.1 -n 127.0.0.1 --talosconfig ./talosconfig
```

And then watch the bootstrap process---just as with the previous command, `talosctl health` assumes port 50000 and does not let you specify a different target port.

```bash
talosctl health -e 127.0.0.1 -n 127.0.0.1 --talosconfig ./talosconfig
```

Once the bootstrap process is complete, then you can retrieve the admin Kubeconfig:

```bash
talosctl kubeconfig -n 127.0.0.1 -e 127.0.0.1 \
    --talosconfig ./talosconfig <output-file-name>
```

All three of the commands you just used assume port 50000 and don't let you specify a different target port.

With the cluster bootstrapped, you can now terminate your existing SSH tunnels and establish new ones to the worker nodes in order to use `talosctl apply-config` with the worker nodes. Applying the machine configuration to the worker nodes will automatically join them to the Kubernetes cluster.

Congratulations! You've just bootstrapped a Talos Linux-powered Kubernetes cluster using SSH.

## Additional Resources

Talos Linux is something I've discussed in podcast episodes and in previous blog posts. Here are some links to additional resources you might find helpful or informative:

[Full Stack Journey 041: Talos Builds an Open Source OS for Kubernetes][link-1]

[Full Stack Journey 082: Inside Talos Linux][link-2]

[Creating a Talos Linux cluster on AWS with Pulumi][xref-1]

[Creating a Talos Linux cluster on Azure with Pulumi][xref-2]

The process described in this post and the commands used were derived from information presented in the Talos Linux documentation; specifically [this page][link-5] and [this page][link-6].

I hope this proves useful to someone out there. If you have any questions, I encourage you to reach out to me---you can reach me [on Twitter][link-7], on [the Fediverse][link-8], or in a number of different Slack communities (the Kubernetes Slack instance is one good example). Even my e-mail address isn't too hard to find, in the event you'd like to drop me a message. Thanks for reading!

[link-1]: https://packetpushers.net/podcasts/full-stack-journey/full-stack-journey-041-talos-builds-an-open-source-os-for-kubernetes/
[link-2]: https://packetpushers.net/podcasts/full-stack-journey/full-stack-journey-082-inside-talos-linux-the-distro-built-for-kubernetes/
[link-3]: https://www.talos.dev/
[link-4]: https://www.talos.dev/v1.7/reference/configuration/v1alpha1/config/#Config.machine
[link-5]: https://www.talos.dev/v1.7/talos-guides/install/cloud-platforms/aws/
[link-6]: https://www.talos.dev/v1.7/introduction/prodnotes/
[link-7]: https://twitter.com/scott_lowe
[link-8]: https://fosstodon.org/@scottslowe
[xref-1]: {{< relref "2023-02-24-creating-a-talos-linux-cluster-on-aws-with-pulumi.md" >}}
[xref-2]: {{< relref "2023-03-31-creating-a-talos-linux-cluster-on-azure-with-pulumi.md" >}}
[xref-3]: {{< relref "2015-11-21-using-ssh-bastion-host.md" >}}
