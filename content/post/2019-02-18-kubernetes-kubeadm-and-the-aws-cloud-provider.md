---
author: slowe
categories: Explanation
comments: true
date: 2019-02-18T12:00:00Z
tags:
- AWS
- CLI
- Kubeadm
- Kubernetes
title: Kubernetes, Kubeadm, and the AWS Cloud Provider
url: /2019/02/18/kubernetes-kubeadm-and-the-aws-cloud-provider/
---

Over the last few weeks, I've noticed quite a few questions appearing in the Kubernetes Slack channels about how to use `kubeadm` to configure Kubernetes with the AWS cloud provider. You may recall that I wrote a post about [setting up Kubernetes with the AWS cloud provider][xref-1] last September, and that post included a few snippets of YAML for `kubeadm` config files. Since I wrote that post, the `kubeadm` API has gone from v1alpha2 (Kubernetes 1.11) to v1alpha3 (Kubernetes 1.12) and now v1beta1 (Kubernetes 1.13). The changes in the `kubeadm` API result in changes in the configuration files, and so I wanted to write this post to explain how to use `kubeadm` 1.13 to set up a Kubernetes cluster with the AWS cloud provider.<!--more-->

I'd recommend reading [the previous post from last September][xref-1] first. In that post, I listed four key configuration items that are necessary to make the AWS cloud provider work:

1. Correct hostname (must match the EC2 Private DNS entry for the instance)
2. Proper IAM role and policy for Kubernetes control plane nodes and worker nodes
3. Kubernetes-specific tags on resources needed by the cluster
4. Correct command-line flags added to the Kubernetes API server, controller manager, and the Kubelet

Items 1-3 remain the same, so I won't discuss them here (refer back to [last September's post][xref-1], where I do provide details). Instead, I'll focus on item 4; in particular, how to use `kubeadm` configuration files to make sure that Kubernetes components are properly configured.

Let's start with the control plane.

## Control Plane Configuration via Kubeadm

I'll assume a highly available (HA) control plane (aka multi-master) configuration here. In this context, "highly available" means multiple control plane nodes and a separate, dedicated etcd cluster.

First, I'll show you a `kubeadm` configuration file that will work, and then I'll walk through some of the key points about the configuration:

``` yaml
---
apiVersion: kubeadm.k8s.io/v1beta1
kind: ClusterConfiguration
apiServer:
  extraArgs:
    cloud-provider: aws
clusterName: test
controlPlaneEndpoint: k8s-elb-1152547997.us-west-2.elb.amazonaws.com
controllerManager:
  extraArgs:
    cloud-provider: aws
    configure-cloud-routes: "false"
    address: 0.0.0.0
etcd:
  external:
    endpoints:
    - https://172.31.33.122:2379
    - https://172.31.42.99:2379
    - https://172.31.46.92:2379
    caFile: /etc/kubernetes/pki/etcd/ca.crt
    certFile: /etc/kubernetes/pki/apiserver-etcd-client.crt
    keyFile: /etc/kubernetes/pki/apiserver-etcd-client.key
kubernetesVersion: v1.13.2
networking:
  dnsDomain: cluster.local
  podSubnet: 192.168.0.0/16
  serviceSubnet: 10.96.0.0/12
scheduler:
  extraArgs:
    address: 0.0.0.0
---
apiVersion: kubeadm.k8s.io/v1beta1
kind: InitConfiguration
nodeRegistration:
  kubeletExtraArgs:
    cloud-provider: aws
```

A few notes about this configuration:

* It should (hopefully) be obvious that you'll need to edit many of these values so they are correct for your specific configuration. Notably, `clusterName`, `controlPlaneEndpoint`, and `etcd.external.endpoints` will all need to be change to the correct values for your environment.
* Whatever DNS name you supply for `controlPlaneEndpoint`---and it should be a DNS name and not an IP address, since in an HA configuration this value should point to a load balancer, and IP addresses assigned to AWS ELBs _can_ change--will also be added as a Subject Alternative Name (SAN) to the API server's certificate. If you have other SANs you want added (like maybe a CNAME alias for the ELB's DNS name), you'll need to manually add those via the `apiServer.certSANs` value (not shown above since it's not needed).
* `kubeadm` normally binds the controller manager and scheduler to localhost, which may cause problems with Prometheus (it won't be able to connect and will think these components are "missing"). The `controllerManager.extrArgs.address` and `scheduler.extraArgs.address` values fix this issue.

You'd use this configuration file as outlined in [this tutorial from the v1.11 version of the Kubernetes docs][link-7]. Why the v1.11 version? In versions 1.12 and 1.13, `kubeadm` introduced a new flag, the `--experimental-control-plane` flag, that allows you to more easily join new control plane nodes to the cluster. The YAML configuration file outlined above won't work with the `--experimental-control-plane` flag. ~~I'm still working through the details and performing some testing, and will update this article with more information once I've worked it out.~~ (The "TL;DR" from the linked v1.11 tutorial is that you'll use `kubeadm init` on each control plane node.) If you _do_ want to use the new functionality represented by the `--experimental-control-plane` flag, see [this post][xref-2].

At the end of the `kubeadm init` commands you'll run to establish the control plane, `kubeadm` will display a command that includes a token and a SHA256 hash. Make note of these values, as you'll need them shortly.

## Worker Node Configuration via Kubeadm

As I did in the control plane section, I'll first present a working `kubeadm` configuration file, then walk through any important points or notes about it:

``` yaml
---
apiVersion: kubeadm.k8s.io/v1beta1
kind: JoinConfiguration
discovery:
  bootstrapToken:
    token: y6yaev.8dvwxx5ny3ef7vlq
    apiServerEndpoint: "k8s-elb-1152547997.us-west-2.elb.amazonaws.com:6443"
    caCertHashes:
      - "sha256:7c9b814812fcba3745744dd45825f61318f7f6ca80018dee453e4b9bc7a5c814"
nodeRegistration:
  name: ip-10-11-12-13.us-west-2.compute.internal
  kubeletExtraArgs:
    cloud-provider: aws
```

As with the configuration file for the control plane, there are a few values you must modify for your particular environment:

* The `token`, `apiServerEndpoint`, and `caCertHashes` values must be set. These values are displayed by `kubeadm init` when establishing the control plane; just plug in those values here.
* The `nodeRegistration.name` value should reflect the correct hostname of the instance being joined to the cluster (which should match the EC2 Private DNS entry for the instance).

Once you've set these values, you'd use this file by running `kubeadm join --config <filename>.yaml` after your control plane is up and running.

Once the process completes, you can run `kubectl get nodes` to verify that the worker node has been added to the cluster. You can verify the operation of the AWS cloud provider by running `kubectl get node <new-nodename> -o yaml` and looking for the presence of the "providerID" field. If that field is there, the cloud provider is working. If the field is not present, the cloud provider isn't working as expected, and it's time to do some troubleshooting.

## More Resources

I gathered the information found in this article from a variety of sources, including my own direct personal experience in testing various configuration parameters and scenarios. Some of the information sources I used include:

* The `kubeadm` v1beta1 [API documentation][link-1]
* The configuration "file" used by the AWS Cluster API provider (see [here][link-2] for the control plane, lines 89 through 111; see [here][link-3] for worker nodes, lines 27 through 39)
* The [Heptio AWS QuickStart][link-5]; in particular, the [script that sets up the Kubernetes control plane][link-6]

I'd also like to include a quick shout-out to the members of the VMware Kubernetes Architecture team (née Heptio Field Engineering), who provided valuable feedback on the information found in this post.

For a vSphere version of this article, I recommend Myles Gray's post on [setting up Kubernetes and the vSphere cloud provider][link-4] (I consulted with Myles on some of the `kubeadm` configurations in his post). Note that Myles' article is still using the "v1alpha3" API version for joining the nodes to the cluster. I'd recommend migrating to the "v1beta1" API if at all possible.

Thanks for reading! If you have any questions, comments, suggestions, or corrections, feel free to [reach out to me on Twitter][link-99]. I'd love to hear from you.

[link-1]: https://godoc.org/k8s.io/kubernetes/cmd/kubeadm/app/apis/kubeadm/v1beta1
[link-2]: https://github.com/kubernetes-sigs/cluster-api-provider-aws/blob/master/pkg/cloud/aws/services/userdata/controlplane.go
[link-3]: https://github.com/kubernetes-sigs/cluster-api-provider-aws/blob/master/pkg/cloud/aws/services/userdata/node.go
[link-4]: https://blah.cloud/kubernetes/setting-up-k8s-and-the-vsphere-cloud-provider-using-kubeadm/
[link-5]: https://github.com/heptio/aws-quickstart
[link-6]: https://github.com/heptio/aws-quickstart/blob/master/scripts/setup-k8s-master.sh.in
[link-7]: https://v1-11.docs.kubernetes.io/docs/setup/independent/high-availability/
[link-99]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2018-09-28-setting-up-the-kubernetes-aws-cloud-provider.md" >}}
[xref-2]: {{< relref "2019-04-16-using-kubeadm-add-new-control-plane-nodes-aws-integration.md" >}}
