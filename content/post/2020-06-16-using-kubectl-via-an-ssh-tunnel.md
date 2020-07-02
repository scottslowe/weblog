---
author: slowe
categories: Tutorial
comments: true
date: 2020-06-16T09:00:00-07:00
tags:
- Kubernetes
- SSH
title: Using kubectl via an SSH Tunnel
url: /2020/06/16/using-kubectl-via-an-ssh-tunnel/
---

In this post, I'd like to share one way (not the only way!) to use `kubectl` to access your [Kubernetes][link-4] cluster via an SSH tunnel. In the future, I may explore some other ways (hit [me on Twitter][link-99] if you're interested). I'm sharing this information because I suspect it is not uncommon for folks deploying Kubernetes on the public cloud to want to deploy them in a way that does not expose them to the Internet. Given that the use of SSH bastion hosts is not uncommon, it seemed reasonable to show how one could use an SSH tunnel to reach a Kubernetes cluster behind an SSH bastion host.<!--more-->

If you're unfamiliar with SSH bastion hosts, see [this post][xref-4] for an overview.

To use `kubectl` via an SSH tunnel through a bastion host to a Kubernetes cluster, there are two steps required:

1. The Kubernetes API server needs an appropriate Subject Alternative Name (SAN) on its certificate.
2. The Kubeconfig file needs to be updated to reflect the tunnel details.

## Ensuring an Appropriate SAN for the API Server

As is the case with just about any TLS-secured connection, if the destination to which you're connecting with `kubectl` doesn't match any of the SANs on the API server's certificate, the `kubectl` commands will fail with an error (server name mismatch or similar). In the case of wanting to use an SSH tunnel with `kubectl`, this means that the API server certificate is going to need a SAN entry for `127.0.0.1`. Why `127.0.0.1`? Although there are several different ways to use SSH tunnels, in this instance you're going to take a local port and forward that local port across the tunnel to a remote system and remote port. Thus, `kubectl` will be talking to a local port (a port listening on `127.0.0.1`)---and now you see why this SAN is needed on the API server.

For existing clusters, this means you'll have to go back and [add a name to the Kubernetes API server certificate][xref-1].

For new clusters, you can "bake" the extra SAN in easily with a `kubeadm` configuration file. This YAML snippet shows how:

```yaml
apiVersion: kubeadm.k8s.io/v1beta2
kind: ClusterConfiguration
apiServer:
  certSANs:
  - "127.0.0.1"
```

(See [here][link-1] for the full reference of the `kubeadm` v1beta2 API.)

For new workload clusters spawned by [Cluster API][link-2], you can add the SAN via the KubeadmConfigSpec, part of the KubeadmControlPlane object, as shown in this YAML (this is for CAPI v1alpha3):

```yaml
apiVersion: controlplane.cluster.x-k8s.io/v1alpha3
kind: KubeadmControlPlane
spec:
  kubeadmConfigSpec:
    clusterConfiguration:
      apiServer:
        certSANs:
        - "127.0.0.1"
```

Regardless of the method you use, the commands outlined in [this article][xref-2] and [this article][xref-3] can be re-purposed to help you verify that `127.0.0.1` is indeed listed as a SAN on the API server's certificate.

One the API server's certificate is correctly configured, then you're ready for step 2---updating the Kubeconfig.

## Updating the Kubeconfig with SSH Tunnel Information

The change to the Kubeconfig file for your cluster is pretty straightforward:

```yaml
apiVersion: v1
kind: Config
clusters:
- cluster:
    certificate-authority-data: <redacted>
    #server: https://api.server.address:6443
    server: https://127.0.0.1:12345
```

Substitute the `12345` in the command above for whatever local port you're going to forward across the SSH tunnel. Don't worry; you can change this later without any major ramifications (the API server certificate doesn't have any port information, so this is easily changed as needed). I prefer to leave the original `server` line in the file but commented out, just in case I need that information later.

Once the SAN entry for `127.0.0.1` is on the API server certificate and your local Kubeconfig file has been updated, then it is just a matter of opening the SSH tunnel:

    ssh -fNT -L 12345:api.server.address:6443 user@ssh-bastion-host

_(Your `ssh` parameters may be slightly different, depending on your SSH version and OS.)_

Again, change `12345` to match whatever you specified in the Kubeconfig file. After you've established the tunnel, then running `kubectl` commands should work without any issues. Voila!

As I mentioned at the start of this post, this is just one way of using SSH to help access a Kubernetes cluster that isn't otherwise directly accessible. There are other ways! If you're interested in having me explore some of those other ways in future posts, let me know---either find [me on Twitter][link-99] or on [the Kubernetes Slack community][link-2].

[link-1]: https://godoc.org/k8s.io/kubernetes/cmd/kubeadm/app/apis/kubeadm/v1beta2
[link-2]: https://cluster-api.sigs.k8s.io/
[link-3]: https://kubernetes.slack.com/
[link-4]: https://kubernetes.io/
[link-99]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2019-07-30-adding-a-name-to-kubernetes-api-server-certificate.md" >}}
[xref-2]: {{< relref "2018-08-20-troubleshooting-tls-certificates.md" >}}
[xref-3]: {{< relref "2018-06-12-examining-x509-certificates-embedded-in-kubeconfig-files.md" >}}
[xref-4]: {{< relref "2015-11-21-using-ssh-bastion-host.md" >}}
