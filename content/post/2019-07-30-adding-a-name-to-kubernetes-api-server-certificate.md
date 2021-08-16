---
author: slowe
categories: Tutorial
comments: true
date: 2019-07-30T12:00:00Z
tags:
- Kubernetes
- CLI
- Linux
- Encryption
title: Adding a Name to the Kubernetes API Server Certificate
url: /2019/07/30/adding-a-name-to-kubernetes-api-server-certificate/
---

In this post, I'm going to walk you through how to add a name (specifically, a Subject Alternative Name) to the TLS certificate used by the Kubernetes API server. This process of updating the certificate to include a name that wasn't included could find use for a few different scenarios. A couple of situations come to mind, such as adding a load balancer in front of the control plane, or using a new or different URL/hostname used to access the API server (both situations taking place _after_ the cluster was bootstrapped).<!--more-->

This process does assume that the cluster was bootstrapped using `kubeadm`. This could've been a simple `kubeadm init` with no customization, or it could've been using a configuration file to modify the behavior of `kubeadm` when bootstrapping the cluster. This process also assumes your Kubernetes cluster is using the default certificate authority (CA) created by `kubeadm` when bootstrapping a cluster. Finally, this process assumes you are using a non-HA (single control plane node) configuration.

Before getting into the details of how to update the certificate, I'd like to first provide a bit of background on why this is important.

## Background

The Kubernetes API server uses digital certificates to both encrypt traffic to/from the API server as well as to authenticate connections to the API server. As such, if you try to connect to the API server using a command-line client like `kubectl` and you use a hostname or IP address that isn't included in the certificate's list of Subject Alternative Names (SANs), you'll get an error indicating that the certificate isn't valid for the specified IP address or hostname. To fix that, you need to update the certificate so that the list of SANs includes any and all IP addresses or hostnames that you will use to access the API server.

## Updating the API Server's Certificate

Because the cluster was bootstrapped using `kubeadm`, you can use `kubeadm` to update the API server's certificate to include additional names in the list of SANs.

To do this, you'll first need a `kubeadm` configuration file. If you used a configuration file to bootstrap the cluster, you can use that file as your starting point. If you didn't use a configuration file---perhaps you just did a quick `kubeadm init` to quickly get a cluster---then you can pull the `kubeadm` configuration from the cluster to create a configuration file you can use. `kubeadm` writes its configuration into a ConfigMap named "kubeadm-config" found in the "kube-system" namespace.

To pull the `kubeadm` configuration from the cluster into an external file, run this command:

    kubectl -n kube-system get configmap kubeadm-config -o jsonpath='{.data.ClusterConfiguration}' > kubeadm.yaml

This creates a file named `kubeadm.yaml`, the contents of which may look something like this (the file from your cluster may have different values):

```yaml
apiServer:
  extraArgs:
    authorization-mode: Node,RBAC
  timeoutForControlPlane: 4m0s
apiVersion: kubeadm.k8s.io/v1beta1
certificatesDir: /etc/kubernetes/pki
clusterName: kubernetes
controlPlaneEndpoint: ""
controllerManager: {}
dns:
  type: CoreDNS
etcd:
  local:
    dataDir: /var/lib/etcd
imageRepository: k8s.gcr.io
kind: ClusterConfiguration
kubernetesVersion: v1.14.4
networking:
  dnsDomain: cluster.local
  podSubnet: ""
  serviceSubnet: 10.96.0.0/12
scheduler: {}
```

In this particular example, no additional SANs are listed. To add at least one SAN, add a `certSANs` list under the `apiServer` section. If you already had a `kubeadm` configuration file that you used when you bootstrapped the cluster, you may already have a `certSANs` list. If not, you'll need to add it; if so, you'll just add another entry to that list.

Here's an example (here I'm showing _only_ the `apiServer` section):

```yaml
apiServer:
  certSANs:
  - "172.29.50.162"
  - "k8s.domain.com"
  - "other-k8s.domain.net"
  extraArgs:
    authorization-mode: Node,RBAC
  timeoutForControlPlane: 4m0s
```

This change to the `kubeadm` configuration file _adds_ SANs for the IP address 172.29.50.162, the hostname "k8s.domain.com", and the hostname "other-k8s.domain.net". This would be _in addition to_ the standard list of SANs that are included (which would be the local hostname, some names for the default Kubernetes Service object, the default IP address for the Kubernetes Service object, and the primary IP address of the node).

Once you've updated the `kubeadm` configuration file---either the default one pulled from the ConfigMap or the custom one you used when you bootstrapped the cluster---then you're ready to update the certificate.

First, move the existing API server certificate and key (if `kubeadm` sees that they already exist in the designated location, it won't create new ones):

    mv /etc/kubernetes/pki/apiserver.{crt,key} ~

Then, use `kubeadm` to _just_ generate a new certificate:

    kubeadm init phase certs apiserver --config kubeadm.yaml

This command will generate a new certificate and key for the API server, using the specified configuration file for guidance. Since the specified configuration file includes a `certSANs` list, then `kubeadm` will automatically add those SANs when creating the new certificate.

The final step is restarting the API server to pick up the new certificate. The easiest way to do this is to kill the API server container using `docker`:

1. Run `docker ps | grep kube-apiserver | grep -v pause` to get the container ID for the container running the Kubernetes API server. (The container ID will be the very first field in the output.)
2. Run `docker kill <containerID>` to kill the container.

If your nodes are running containerd as the container runtime, the commands are a bit different:

1. Run `crictl pods | grep kube-apiserver | cut -d' ' -f1` to get the Pod ID for the Kubernetes API server Pod.
2. Run `crictl stopp <pod-id>` to stop the Pod.
3. Run `crictl rmp <pod-id>` to remove the Pod.

The Kubelet will automatically restart the container, which will pick up the new certificate. As soon as the API server restarts, you will immediately be able to connect to it using one of the newly-added IP addresses or hostnames.

## Verifying the Change

One way to verify the change is to edit the `server` line for that cluster in your Kubeconfig file to use one of the newly-added IP addresses or hostnames, and then run `kubectl` against the cluster. If `kubectl` doesn't work, then you can start troubleshooting from there.

Another way to verify the change---and a handy troubleshooting step as well---is to use `openssl` on the Kubernetes control plane node to decode the certificate and show the list of SANs on the certificate:

    openssl x509 -in /etc/kubernetes/pki/apiserver.crt -text

Look for the "X509v3 Subject Alternative Name" line, after which will be a list of all the DNS names and IP addresses that are included on the certificate as SANs. After following this procedure, you _should_ see the newly-added names and IP addresses you specified in the modified `kubeadm` configuration file. If you don't, then something went wrong along the way. Common mistakes in this process include forgetting to remove the previous certificate and key (`kubeadm` won't create new ones if they already exist), or failing to include the `--config kubeadm.yaml` on the `kubeadm init phase certs` command.

## Updating the In-Cluster Configuration

Assuming everything is working as expected, the final step is to update the `kubeadm` ConfigMap stored in the cluster. This is important so that when you use `kubeadm` to orchestrate a cluster upgrade later, the updated information will be present in the cluster. Thankfully, this is pretty straightforward:

    kubeadm config upload from-file --config kubeadm.yaml

(Newer versions of Kubernetes use the command `kubeadm init phase upload-config kubeadm --config kubeadm.yaml`. I _believe_ this change takes effect around the Kubernetes 1.15 release, but I may be mistaken. Thanks Sarye Haddadi for the correction!)

You can verify the changes to the configuration were applied successfully with this command:

    kubectl -n kube-system get configmap kubeadm-config -o yaml

## Final Thoughts

One other use case I wanted to mention briefly in addition to the ones mentioned earlier---adding a load balancer in front of the control plane node or adding a new DNS name or IP address used to reference the control plane endpoint---is enabling the use of SSH tunneling to access the control plane endpoint. In those cases, you'll want to add "127.0.0.1" as a SAN to the certificate. When using SSH tunneling, you'll be taking port 6443 (or whatever port you're using) on 127.0.0.1 and sending it across to the control plane node via your SSH connection. By including "127.0.0.1" as a SAN on the API server certificate, you'll enable this usage pattern.

That's it---I hope this is helpful in some fashion. If you have any questions, comments, suggestions, or corrections, [hit me up on Twitter][link-1].

[link-1]: https://twitter.com/scott_lowe
