---
author: slowe
categories: Education
comments: true
date: 2021-08-03T15:25:00-06:00
tags:
- CLI
- etcd
- Kubeadm
- Kubernetes
- Linux
title: An Alternate Approach to etcd Certificate Generation with Kubeadm
url: /2021/08/03/alternate-approach-etcd-certificate-generation-kubeadm/
---

I've written a fair amount about `kubeadm`, which was my preferred way of bootstrapping [Kubernetes][link-1] clusters until [Cluster API][link-2] arrived. Along the way, I've also discussed using `kubeadm` to assist with setting up etcd, the distributed key-value store leveraged by the Kubernetes control plane (see [here][xref-1], [here][xref-2], and [here][xref-3]). In this post, I'd like to revisit the topic of using `kubeadm` to set up an etcd cluster once again, this time taking a look at an alternate approach to generating the necessary TLS certificates than what the official documentation describes.<!--more-->

There is absolutely nothing wrong with the process the official documentation describes (I'm referring to [this page][link-3], by the way); this process just creates slightly "cleaner" certificates. What do I mean by "cleaner" certificates? The official documentation uses a series of `kubeadm` configuration files, one for each etcd cluster member, to control how the utility creates the necessary certificates and configuration files. The user is instructed to use these configuration files _on a single system_ to generate the certificates for all the cluster members. This works fine, with one caveat: each of the certificates will have an extra hostname---the hostname of the system being used to generate the certificates---encoded in the certificate.

Instead of generating all the certificates on a single host, I prefer to generate the certificates on each host. This does require distributing the CA certificate and key to all three hosts, but it avoids the extraneous hostname on the certificates that results from generating all the certificates on the same host. One could also remove the CA key from the hosts after the certificates are generated, if there were concerns about it being copied too widely.

Here's the `kubeadm` configuration file I use, which is only very lightly modified from the one in the official documentation:

```yaml
apiVersion: kubeadm.k8s.io/v1beta2
kind: ClusterConfiguration
etcd:
  local:
    serverCertSANs:
    - "FQDN"
    peerCertSANs:
    - "FQDN"
    extraArgs:
      initial-cluster: ip-10-11-12-154=https://ip-10-11-12-154.us-west-2.compute.internal:2380,ip-10-11-48-148=https://ip-10-11-48-148.us-west-2.compute.internal:2380,ip-10-11-96-141=https://ip-10-11-96-141.us-west-2.compute.internal:2380
      initial-cluster-state: new
      name: HOST
      listen-peer-urls: https://IP:2380
      listen-client-urls: https://IP:2379
      advertise-client-urls: https://FQDN:2379
      initial-advertise-peer-urls: https://FQDN:2380
```

_(Note: the IP addresses above have been randomized and do not refer to actual instances in my account.)_

This configuration file is lightly modified to use fully-qualified domain names (FQDNs) where possible/supported by etcd. The `listen-peer-urls` and `listen-client-urls` settings require an IP address, but the other settings support FQDNs.

Aside from populating the `initial-cluster` line---which has to list all the etcd cluster members---I copy this file unchanged to each host. On each host, I then customize the file using these commands:

    sed -i "s/FQDN/$(hostname -f)/g" kubeadm.yaml
    sed -i "s/IP/$(hostname -i)/g" kubeadm.yaml
    sed -i "s/HOST/$(hostname -s)/g" kubeadm.yaml

Once the configuration file is appropriately customized for that particular host, then it's just a matter of running the same commands as in the official documentation, just with slightly different parameters:

    kubeadm init phase certs etcd-server --config=kubeadm.yaml
    kubeadm init phase certs etcd-peer --config=kubeadm.yaml
    kubeadm init phase certs etcd-healthcheck-client --config=kubeadm.yaml
    kubeadm init phase certs apiserver-etcd-client --config=kubeadm.yaml
    kubeadm init phase etcd local --config=kubeadm.yaml

Run these commands on each node, and you should end up with a working three-node etcd cluster.

One other advantage of doing it this way is the potential for automation---the source config file is the same and can be distributed unchanged to each node, and the commands that are run on each host are the same and can be easily scripted (in fact, in some cases I've put all these commands into a quick-and-dirty shell script).

Hit [me on Twitter][link-4] if you have any questions or any feedback. Thanks for reading!

[link-1]: https://kubernetes.io
[link-2]: https://cluster-api.sigs.k8s.io
[link-3]: https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/setup-ha-etcd-with-kubeadm/
[link-4]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2018-08-21-bootstrapping-etcd-cluster-with-tls-using-kubeadm.md" >}}
[xref-2]: {{< relref "2018-10-29-more-on-setting-up-etcd-with-kubeadm.md" >}}
[xref-3]: {{< relref "2020-04-02-setting-up-etcd-with-kubeadm-containerd-edition.md" >}}
