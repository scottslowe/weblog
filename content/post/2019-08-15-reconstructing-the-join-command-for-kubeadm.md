---
author: slowe
categories: Explanation
comments: true
date: 2019-08-15T12:00:00Z
tags:
- CLI
- Kubeadm
- Kubernetes
- Linux
title: Reconstructing the Join Command for Kubeadm
url: /2019/08/15/reconstructing-the-join-command-for-kubeadm/
---

If you've used `kubeadm` to bootstrap a [Kubernetes][link-1] cluster, you probably know that at the end of the `kubeadm init` command to bootstrap the first node in the cluster, `kubeadm` prints out a bunch of information: how to copy over the admin Kubeconfig file, and how to join both control plane nodes and worker nodes to the cluster you just created. But what if you didn't write these values down after the first `kubeadm init` command? How does one go about reconstructing the proper `kubeadm join` command?<!--more-->

Fortunately, the values needed for a `kubeadm join` command are relatively easy to find or recreate. First, let's look at the values that are needed.

Here's the skeleton of a `kubeadm join` command for a control plane node:

    kubeadm join <endpoint-ip-or-dns>:<port> \
    --token <valid-bootstrap-token> \
    --discovery-token-ca-cert-hash <ca-cert-sha256-hash> \
    --control-plane \
    --certificate-key <certificate-key>

And here's the skeleton of a `kubeadm join` command for a worker node:

    kubeadm join <endpoint-ip-or-dns>:<port> \
    --token <valid-bootstrap-token> \
    --discovery-token-ca-cert-hash <ca-cert-sha256-hash> \

As you can see, the information needed for the worker node is a subset of the information needed for a control plane node.

Here's how to find or recreate all the various pieces of information you need:

* The value for `<endpoint-ip-or-dns>:<port>` can be retrieved from the "kubeadm-config" ConfigMap in the cluster's "kube-system" namespace. Just run `kubectl -n kube-system get cm kubeadm-config -o yaml` and then look for the value specified for `controlPlaneEndpoint`.
* For `<valid-bootstrap-token>`, there's no way to find out the original token. It is, however, easy to create a new token. Just run `kubeadm token create` and note the output. Be aware that tokens have a default lifetime of 24 hours.
* For `<ca-cert-sha256-hash>`, see [this blog post][xref-1].
* For `<certificate-key>`, you can't get whatever original value was output by `kubeadm init`. You _can_, though, generate another value. (This value is only good for two hours, so it's far more likely you'd need to generate another value anyway.) Run `kubeadm init phase upload-certs --upload-certs` and make a note of the output of the command.

There you have it---how to easily reconstruct all the information needed to use `kubeadm join` to join a node (either control plane node or worker node) to a Kubernetes cluster. Note that this information also applies to gathering the information needed for a JoinConfiguration stanza in a `kubeadm` configuration file, like one used in [this post][xref-2] to join control plane nodes or worker nodes to a cluster.

If I've missed anything here, feel free to [contact me on Twitter][link-2]. Thanks!

[link-1]: https://kubernetes.io/
[link-2]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2019-07-12-calculating-ca-certificate-hash-for-kubeadm.md" >}}
[xref-2]: {{< relref "2019-08-14-setting-up-aws-integrated-kubernetes-115-cluster-kubeadm.md" >}}
