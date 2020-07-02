---
author: slowe
categories: Tutorial
comments: true
date: 2020-04-23T20:30:00-07:00
tags:
- etcd
- CLI
- Linux
title: Setting up etcd with etcdadm
url: /2020/04/23/setting-up-etcd-with-etcdadm/
---

I've written a few different posts on setting up [etcd][link-1]. There's this one on [bootstrapping a TLS-secured etcd cluster with `kubeadm`][xref-1], and there's this one about [using `kubeadm` to run an etcd cluster as static Pods][xref-2]. There's also this one about [using `kubeadm` to run etcd with containerd][xref-3]. In this article, I'll provide yet another way of setting up a "best practices" etcd cluster, this time using a tool named `etcdadm`.<!--more-->

`etcdadm` is [an open source project][link-2], originally started by Platform9 (here's [the blog post][link-3] announcing the project being open sourced). As the README in the GitHub repository mentions, the user experience for `etcdadm` "is inspired by `kubeadm`."

## Getting `etcdadm`

The instructions in the repository indicate that you can use `go get -u sigs.k8s.io/etcdadm`, but I ran into problems with that approach (using Go 1.14). At the suggestion of the one of the maintainers, I also tried Go 1.12, but it failed both on my main Ubuntu laptop as well as on a clean Ubuntu VM. However, running `make etcdadm` in a clone of the repository worked, and one of the maintainers indicated the documentation will be updated to reflect this approach instead. So, for now, it would appear that cloning the GitHub repository and running `make etcdadm` is your best approach for obtaining a binary build of the tool.

## Setting up an Etcd Cluster

Once you have a binary build of `etcdadm`, the process for setting up an etcd cluster is---as the documentation in the repository indicates---pretty straightforward.

1. First, copy the `etcdadm` binary to the system(s) that will comprise the cluster.
2. On the system that will be the first node in the cluster, run `etcdadm init`. This will create a certificate authority (CA) for the TLS certs, then use that CA to create TLS certificates for the etcd node. Finally, it will bootstrap the etcd cluster with one only member, itself, and then will output a command to run on subsequent nodes. The command will look something like `etcdadm join https://10.11.12.13:2379`.

    As part of the bootstrapping process, `etcdadm` will install the `etcd` and `etcdctl` binaries (the default location is into `/opt/bin`), and will create a systemd unit to run etcd as a service. You can use `systemctl` to check the status of the etcd unit, and you can use `etcdctl` to check the health of the cluster (among other things).
3. Copy the CA certificate and key from the first node to the next node you're going to add to the cluster. The CA certificate and key are found as `/etc/etcd/pki/ca.crt` and `/etc/etcd/pki/ca.key`, respectively. Place these files in the same location on the next node, then run the command output by `etcdadm init` in the previous step.
4. Repeat step 3 for each additional node you add to the cluster, keeping in mind you should use odd numbers of nodes in the cluster. Generally speaking, a cluster of 3 nodes will work just fine in the vast majority of cases.

Once you've added the desired number of nodes to the etcd cluster, you can use this `etcdctl` command to check the health of the cluster:

    ETCDCTL_API=3 /opt/bin/etcdctl --cert /etc/etcd/pki/peer.crt \
    --key /etc/etcd/pki/peer.key --cacert /etc/etcd/pki/ca.crt \
    --endpoints https://10.45.88.87:2379 endpoint health --cluster

Assuming you get healthy responses to the above command, you're good to go. Pretty easy, right? This is why I tweeted earlier today that I think there is real promise in this tool.

## How I Tested

To test `etcdadm`, I first used [Vagrant][link-7] to spin up an Ubuntu 18.04 VM for building the `etcdadm` binary. After I had the binary, I used [Pulumi][link-4] to launch three AWS EC2 instances in their own VPC. I then used these instances to test the process of bootstrapping the etcd cluster using `etcdadm`.

While `etcdadm` is still an early project, I would recommend keeping an eye on its development. In the meantime, I'd love to hear any feedback from you, so feel free to find [me on Twitter][link-5] or hit me up on [the Kubernetes Slack instance][link-6].

[link-1]: https://etcd.io/
[link-2]: https://github.com/kubernetes-sigs/etcdadm/
[link-3]: https://platform9.com/blog/were-open-sourcing-etcdadm-heres-what-it-means-for-kubernetes-in-production/
[link-4]: https://www.pulumi.com/
[link-5]: https://twitter.com/scott_lowe
[link-6]: https://kubernetes.slack.com
[link-7]: https://www.vagrantup.com
[xref-1]: {{< relref "2018-08-21-bootstrapping-etcd-cluster-with-tls-using-kubeadm.md" >}}
[xref-2]: {{< relref "2018-10-29-more-on-setting-up-etcd-with-kubeadm.md" >}}
[xref-3]: {{< relref "2020-04-02-setting-up-etcd-with-kubeadm-containerd-edition.md" >}}
