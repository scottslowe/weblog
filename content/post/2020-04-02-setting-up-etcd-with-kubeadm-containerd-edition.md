---
author: slowe
categories: Tutorial
comments: true
date: 2020-04-02T09:30:00-07:00
tags:
- CLI
- etcd
- Kubeadm
- Kubernetes
- Linux
title: "Setting up etcd with Kubeadm, containerd Edition"
url: /2020/04/02/setting-up-etcd-with-kubeadm-containerd-edition/
---

In late 2018, I wrote a couple of blog posts on using `kubeadm` to set up an etcd cluster. The first one was [this post][xref-1], which used `kubeadm` only to generate the TLS certs but ran etcd as a systemd service. I followed up that up a couple months later with [this post][xref-2], which used `kubeadm` to run etcd as a static Pod on each system. It's that latter post---running etcd as a static Pod on each system in the cluster---that I'll be revisiting in this post, only this time using [containerd][link-3] as the container runtime instead of Docker.<!--more-->

This post assumes you've already created the VMs/instances on which etcd will run, that an appropriate version of Linux is installed (I'll be using Ubuntu LTS 18.04.4), and that the appropriate packages have been installed. This post also assumes that you've already made sure that the correct etcd ports have been opened between the VMs/instances, so that etcd can communicate properly.

Finally, this post builds upon the official Kubernetes documentation on [setting up an etcd cluster using `kubeadm`][link-2]. The official guide assumes the use of Docker, whereas this post will focus on using containerd as the container runtime instead of Docker. The sections below outline the _changes_ required to the official documentation in order to make it work with containerd.

## Configuring Kubelet

The official documentation provides a systemd drop-in to configure the Kubelet to operate in a "stand-alone" mode. Unfortunately, this drop-in won't work with containerd. Here is a replacement drop-in for containerd:

```
[Service]
ExecStart=
ExecStart=/usr/bin/kubelet --address=127.0.0.1 --pod-manifest-path=/etc/kubernetes/manifests --cgroup-driver=systemd --container-runtime=remote --runtime-request-timeout=15m --container-runtime-endpoint=unix:///run/containerd/containerd.sock
Restart=always
```

The changes here are the addition of the `--container-runtime`, `--runtime-remote`, and `--container-runtime-endpoint` parameters. These parameters configure the Kubelet to talk to containerd instead of Docker.

As instructed in the official documentation, put this into a systemd drop-in (like the suggested `20-etcd-service-manager.conf`) and copy it into the `/etc/systemd/system/kubelet.service.d` directory. Once the file is in place, run `systemctl daemon-reload` so that systemd will pick up the changes.

## Create the Manifests Directory

Although this is not included in the official documentation, I saw problems with the Kubelet starting up if the manifests directory doesn't exist. I suggest manually creating the `/etc/kubernetes/manifests` directory to avoid any such issues.

## Bootstrap the etcd Cluster

Aside from the changes/differences described above, the rest of the process is as outlined in the official documentation. At a high-level, that means:

1. Use `kubeadm init phase certs etcd-ca` to generate the etcd CA certificate and key.
2. Use `kubeadm init phase certs` to generate the etcd server, peer, health check, and API server client certificates.
3. Distribute the certificates to the etcd nodes.
4. Use `kubeadm init phase etcd local` to generate the Pod manifests for the etcd static Pods.

One final note: the `docker` command at the end of the official documentation won't work in this case, since containerd is the container runtime instead of Docker. I'm still working on the correct containerd equivalent command to test the health of the cluster.

## How I Tested

I used [Pulumi][link-1] to create a test environment in AWS for testing the instructions in this article. The TypeScript code that I wrote for use with Pulumi creates an environment suitable for use with Kubernetes Cluster API, including a VPC, both public and private subnets, an Internet gateway, NAT gateways for the private subnets, all associated route tables and route table associations, and the necessary security groups. I hope to publish this code for others to use soon; look for an update here.

If you have any questions, concerns, or corrections, please contact me. You can reach [me on Twitter][link-4], or contact me on [the Kubernetes Slack instance][link-5]. I'd love to hear from you, and all constructive feedback is welcome.

[link-1]: https://www.pulumi.com/
[link-2]: https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/setup-ha-etcd-with-kubeadm/
[link-3]: https://containerd.io/
[link-4]: https://twitter.com/scott_lowe
[link-5]: https://kubernetes.slack.com
[xref-1]: {{< relref "2018-08-21-bootstrapping-etcd-cluster-with-tls-using-kubeadm.md" >}}
[xref-2]: {{< relref "2018-10-29-more-on-setting-up-etcd-with-kubeadm.md" >}}
