---
aliases: /2018/08/21/boostrapping-etcd-cluster-with-tls-using-kubeadm/
author: slowe
categories: Tutorial
comments: true
date: 2018-08-21T13:00:00Z
tags:
- CLI
- etcd
- Encryption
- Kubeadm
- Linux
- Security
title: Bootstrapping an etcd Cluster with TLS using Kubeadm
url: /2018/08/21/bootstrapping-etcd-cluster-with-tls-using-kubeadm/
---

The [etcd distributed key-value store][link-4] is an integral part of [Kubernetes][link-5]. I first wrote about etcd back in 2014 in [this post][xref-1], but haven't really discussed it in any great detail since then. However, as part of my recent efforts to dive _much_ deeper into Kubernetes, I needed to revisit etcd. In this post, I wanted to share how to boostrap a new etcd cluster with TLS certificates using `kubeadm`.<!--more-->

Before I go on, I feel compelled to state that this is certainly not the _only_ way to bootstrap an etcd cluster with TLS certificates. I feel I must also state that nothing in what I'm about to share is new, novel, revolutionary, or unusual. In fact, a fair amount of it is based on [these instructions][link-10], although this post will focus on using systemd unit files instead of static pods under Kubernetes. I'm simply documenting it here in the hopes of getting the information more broadly disseminated, and to help document my own journey of learning.

## Preparing the Systems

Before you bootstrap the etcd cluster, you'll first need to prepare the nodes for the process. Although I'll list the steps manually below, in practice you'll want to build some automation tooling to help with this process. (My ["learning-tools" GitHub repository][link-3] has [an example][link-6] that you could use as a starting point for your own tooling.)

1. You'll need to download an etcd release from GitHub.

2. Unpack the release tarball and copy the `etcd` and `etcdctl` binaries to somewhere in your path, like `/usr/local/bin`. (Those two binaries are all you need.)

3. Create a data directory for etcd to use, like `/var/lib/etcd`.

4. Download the `kubeadm` binary and place it somewhere in your path, like `/usr/local/bin`. I prefer downloading the `kubeadm` binary as opposed to using package management, as package management also installs some other packages that aren't needed in this case. Use [this page][link-9] to see how to download `kubeadm` (just substitute `kubeadm` for `kubectl`). Note that this procedure was tested with `kubeadm` version 1.11.2; other versions _should_ work fine but I can't guarantee it.

You'll need to repeat this process for each system that will participate in the etcd cluster.

Once you've completed these steps, then you're ready to proceed with boostrapping the etcd cluster with TLS encryption and TLS client authentication.

## Bootstrapping the etcd Cluster

With all the binaries in place, you'll start by generating all the certificates that are needed. This is where you'll leverage `kubeadm`; specifically, the `kubeadm alpha phase certs` command.

First, create a self-signed CA for etcd:

    kubeadm alpha phase certs etcd-ca

This will create a CA certificate and key in `/etc/kubernetes/pki/etcd`. The files will be named `ca.crt` and `ca.key`. Copy these files to the same location on the other etcd nodes (you may need to create the directory structure first with `mkdir -p`).

Before you can continue with the next steps---generating server and peer certificates---you'll need to first create a configuration file for `kubeadm` that provides some additional instructions on how to create those certificates. Specifically, this configuration file will instruct `kubeadm` on which Subject Alternative Names (SANs) should be included on the certificates.

Here's the configuration file (this is for an EC2 instance in the "us-west-2" region with the private IP address 192.168.1.100, substitute your correct values here):

``` yaml
apiVersion: "kubeadm.k8s.io/v1alpha2"
kind: MasterConfiguration
etcd:
  local:
    serverCertSANs:
      - "ip-192-168-1-100.us-west-2.compute.internal"
      - "192.168.1.100"
    peerCertSANs:
      - "ip-192-168-1-100.us-west-2.compute.internal"
```

This configuration file syntax is specific to 1.11 (and possibly newer); it won't work for 1.10. Also, play close attention to capitalization and spacing (`kubeadm` is very particular).

With the `kubeadm` configuration in place, you can create the etcd server certificate (assuming the configuration file is stored as `etcd.yaml`):

    kubeadm alpha phase certs etcd-server --config etc.yaml

This will create a `server.crt` and `server.key` in `/etc/kubernetes/pki/etcd`. Repeat this process on the other etcd nodes---don't copy certificates across nodes as the hostnames on the certificate won't match up. Make sure you appropriately customize the `etcd.yaml` configuration file for each host so that the hostnames and IP addresses assigned to the certificates are correct.

Next, create the etcd peer certificates:

    kubeadm alpha phase certs etcd-peer --config etcd.yaml

This creates two files (`peer.crt` and `peer.key`) in the same directory as the previous steps (`/etc/kubernetes/pki/etcd`). As with the previous step, repeat this command on the other etcd nodes; don't copy certificates around.

You now have the etcd binaries in place and have all the necessary certificates for securing etcd. The next step is to create the systemd unit file that will start etcd. It should look something like this:

```
[Unit]
Description=etcd
Documentation=https://github.com/coreos/etcd
Conflicts=etcd2.service
Wants=network-online.target network.target
After=network-online.target

[Service]
Type=notify
Restart=always
RestartSec=5s
LimitNOFILE=40000
TimeoutStartSec=0
EnvironmentFile=/etc/etcd/etcd.conf
ExecStart=/usr/local/bin/etcd

[Install]
WantedBy=multi-user.target
```

The systemd unit references an environment file to keep the systemd unit file as simple and straightforward as possible. Here's the environment file that would go along with this systemd unit file:

```
ETCD_NAME="node1"
ETCD_DATA_DIR="/var/lib/etcd"
ETCD_LISTEN_CLIENT_URLS="https://0.0.0.0:2379"
ETCD_LISTEN_PEER_URLS="https://0.0.0.0:2380"
ETCD_ADVERTISE_CLIENT_URLS="https://192.168.1.100:2379"
ETCD_INITIAL_ADVERTISE_PEER_URLS="https://192.168.1.100:2380"
ETCD_INITIAL_CLUSTER="node1=https://192.168.1.100:2380,\
node2=https://192.168.1.101:2380,\
node3=https://192.168.1.102:2380
ETCD_INITIAL_CLUSTER_STATE="new"
ETCD_INITIAL_CLUSTER_TOKEN="new-cluster"
ETCD_TRUSTED_CA_FILE="/etc/kubernetes/pki/etcd/ca.crt"
ETCD_CERT_FILE="/etc/kubernetes/pki/etcd/server.crt"
ETCD_KEY_FILE="/etc/kubernetes/pki/etcd/server.key"
ETCD_CLIENT_CERT_AUTH="true"
ETCD_PEER_TRUSTED_CA_FILE="/etc/kubernetes/pki/etcd/ca.crt"
ETCD_PEER_CERT_FILE="/etc/kubernetes/pki/etcd/peer.crt"
ETCD_PEER_KEY_FILE="/etc/kubernetes/pki/etcd/peer.key"
ETCD_PEER_CLIENT_CERT_AUTH="true"
```

Note that you **must** customize this environment file for each system in the cluster! Be sure to set the `ETCD_NAME`, `ETCD_ADVERTISE_CLIENT_URLS`, and `ETCD_INITIAL_ADVERTISE_PEER_URLS` values to the correct values for each system. Also be sure that the names and IP addresses referenced in `ETCD_INITIAL_CLUSTER` correctly match the names (as specified by `ETCD_NAME`) and IP addresses of the systems in the cluster.

Because you've modified systemd's configuration by adding a new unit file, you'll need to run `systemctl daemon-reload`, and then you can enable and start etcd:

    systemctl enable etcd
    systemctl start etcd

Use `systemctl status etcd` and/or `journalctl -u etcd` to make sure that the cluster formed, a leader was elected, and that etcd is serving client requests.

The final step is just to verify that etcd is working as expected. You can use `etcdctl` to perform this test:

    ETCDCTL_API=3 etcdctl --cacert=/etc/kubernetes/pki/etcd/ca.crt \
    --cert=/etc/kubernetes/pki/etcd/peer.crt \
    --key=/etc/kubernetes/pki/etcd/peer.key \
    --endpoints=https://192.168.1.100:2379,https://192.168.1.101:2379,\
    https://192.168.1.102:2379
    endpoint health

It may be obvious but it's worth pointing out/reminding readers that you'll need to customize the `--endpoints=` parameter to reflect the correct IP addresses of the systems in your cluster.

This `etcdctl` command looks complicated, but it's necessary for a couple reasons (see [here][link-8] for more details on this command):

* This is an etcd version 3 cluster, so you have to explicitly tell `etcdctl` to use the version 3 API.
* Because client certification authentication is enabled, you can't make a connection to the etcd cluster without a client certificate. Hence, you need to specify all the various certificate-related flags.

Assuming the `etcdctl` command worked correctly and reported no errors (all cluster endpoints healthy), then you're all set!

## Additional Resources

Virtually everyone who does content creation stands on the shoulders of those who have gone before them, and I'm no exception. Refer to these resources for more information or additional perspectives on this topic:

[etcd Security Model][link-1]  
[The "Bootstrapping the etcd Cluster" portion of Kelsey Hightower's Kubernetes the Hard Way repository][link-2]

[This GitHub repository][link-7] by my colleague Duffie Cooley also provided some inspiration and guidance for some portions of this process.

**UPDATE 2020-01-31:** Note that the `kubeadm alpha phase` command was replaced by the `kubeadm init phase` command in the Kubernetes 1.13 release. Since readers are pretty likely to be running a release later than 1.13, please replace all instances of `kubeadm alpha phase` above with `kubeadm init phase`. The commands remain otherwise identical.

[link-1]: https://coreos.com/etcd/docs/latest/op-guide/security.html
[link-2]: https://github.com/kelseyhightower/kubernetes-the-hard-way/blob/master/docs/07-bootstrapping-etcd.md
[link-3]: https://github.com/scottslowe/learning-tools/
[link-4]: https://github.com/coreos/etcd/
[link-5]: https://kubernetes.io/
[link-6]: https://github.com/scottslowe/learning-tools/tree/master/etcd/etcdv3-ansible-aws-tf
[link-7]: https://github.com/mauilion/wardroom-nc/
[link-8]: https://github.com/coreos/etcd/tree/master/etcdctl
[link-9]: https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl-binary-via-curl
[link-10]: https://kubernetes.io/docs/setup/independent/setup-ha-etcd-with-kubeadm/
[xref-1]: {{< relref "2014-08-18-coreos-continued-etcd.md" >}}
