---
author: slowe
categories: Tutorial
comments: true
date: 2019-08-14T12:00:00Z
tags:
- AWS
- CLI
- Kubeadm
- Kubernetes
- Linux
title: "Setting up an AWS-Integrated Kubernetes 1.15 Cluster with Kubeadm"
url: /2019/08/14/setting-up-aws-integrated-kubernetes-115-cluster-kubeadm/
---

In this post, I'd like to walk through setting up an AWS-integrated [Kubernetes][link-1] 1.15 cluster using `kubeadm`. Over the last year or so, the power and utility of `kubeadm` has vastly improved (thank you to all the contributors who have spent countless hours!), and it is now---in my opinion, at least---at a point where setting up a well-configured, highly available Kubernetes cluster is pretty straightforward.<!--more-->

This post builds on [the official documentation][link-2] for setting up a highly available Kubernetes 1.15 cluster. This post also builds upon previous posts I've written about setting up Kubernetes clusters with the AWS cloud provider:

* [Setting up the AWS Cloud Provider][xref-1]
* [Kubernetes, Kubeadm, and the AWS Cloud Provider][xref-2]
* [Using Kubeadm to Add New Control Plane Nodes with AWS Integration][xref-3]

All of these posts are focused on Kubernetes releases prior to 1.15, and given the changes in `kubeadm` in the 1.14 and 1.15 releases, I felt it would be helpful to revisit the process again for 1.15. For now, I'm focusing on the in-tree AWS cloud provider; however, in the very near future I'll look at using the new external AWS cloud provider.

As pointed out in [the "original" September 2018 article][xref-1], there are a number of high-level requirements needed in order to properly bootstrap an AWS-integrated Kubernetes cluster:

1. The hostname of each node must match the EC2 Private DNS entry for the instance. By default this is something like `ip-10-11-12-13.us-west-2.compute.internal`, but the use of custom DNS domains is possible (a teammate has verified this and hopefully will be blogging about it soon).
2. Each node must have an IAM instance profile that grants it access to an IAM role and policy with permissions to the AWS API.
3. Resources used by the cluster must have specific AWS tags assigned to them.
4. Specific entries are needed in the `kubeadm` configuration file used to bootstrap the cluster, join control plane nodes, and join worker nodes.

Let's dig into each of these areas in a bit more detail. (Most, if not all, of this information is also included in [the September 2018 post][xref-1]. Feel free to check that post for more information or details.)

## Setting Node Hostnames

The hostname for the OS _must_ match the EC2 Private DNS entry for that particular instance. By default, this is typically something like `ip-10-11-12-13.us-west-2.compute.internal` (change the numbers and the region to appropriately reflect the private IP address and region of the instance). The fastest/easiest way I've verified to make sure this is the case is with this command:

    sudo hostnamectl set-hostname \
    $(curl -s http://169.254.169.254/latest/meta-data/local-hostname)

Be sure to set the hostname before starting the bootstrapping process. Anecdotally, I've heard of some success putting this command in the user data for the instance, so that it runs automatically.

## Creating and Assigning the IAM Instance Profile

The nodes in the cluster need to have specific permissions to AWS API objects in order for the AWS cloud provider to work. [The `cloud-provider-aws` GitHub repository][link-4] has full details on the specific permissions needed for control plane nodes and worker nodes. You'll want to create the appropriate IAM roles, IAM policies, and IAM instance profiles so that the nodes have the necessary access. When creating the instances (either via the console, CLI, or some infrastructure-as-code tool), be sure to specify the IAM instance profile they should have assigned.

## Assigning Tags to Resources

The AWS cloud provider needs resources to be tagged with a tag named `kubernetes.io/cluster/<cluster-name>` (the value is immaterial). Based on all the documentation I've been able to find, this tag is needed on all nodes, on exactly one security group (the nodes should be a member of this security group), and on all subnets and route tables. Failing to have things properly tagged will result in odd failure modes, like ELBs being automatically created in response to the creation of a Service object of type `LoadBalancer`, but instances never being populated for the ELB (for example).

## Using Kubeadm Configuration Files

To use `kubeadm` to bootstrap a Kubernetes 1.15 cluster with the AWS cloud provider, you'll want to use a `kubeadm` configuration file (Kubernetes 1.15 uses the "v1beta2" version of the `kubeadm` API; documentation for the API is [here][link-3]).

Three different configuration files are needed:

1. A configuration file to be used to bootstrap the first/initial control plane node
2. A configuration file used to join the additional control plane nodes
3. A Configuration file used to join worker nodes

The sections below provide details on each of these configuration files, along with examples you can use to create your own.

### Bootstrapping the First Control Plane Node

Here's an example of a `kubeadm` configuration file you could use to bootstrap the first control plane node (some explanation is found below the example):

```yaml
---
apiVersion: kubeadm.k8s.io/v1beta2
kind: ClusterConfiguration
apiServer:
  extraArgs:
    cloud-provider: aws
clusterName: blogsample
controlPlaneEndpoint: cp-lb.us-west-2.elb.amazonaws.com
controllerManager:
  extraArgs:
    cloud-provider: aws
    configure-cloud-routes: "false"
kubernetesVersion: stable
networking:
  dnsDomain: cluster.local
  podSubnet: 192.168.0.0/16
  serviceSubnet: 10.96.0.0/12
---
apiVersion: kubeadm.k8s.io/v1beta2
kind: InitConfiguration
nodeRegistration:
  kubeletExtraArgs:
    cloud-provider: aws
```

Let's break this down just a bit:

* The `apiServer.extraArgs.cloud-provider` and `controllerManager.extraArgs.cloud-provider` settings are what generate the `--cloud-provider=aws` flags for the API server and the controller manager (you can verify this by looking at their manifests in `/etc/kubernetes/manifests` after bootstrapping the control plane node). You need these settings here because the AWS cloud provider needs to be specified when you first bootstrap the node, not added afterward.
* The `nodeRegistration.kubeletExtraArgs.cloud-provider` setting serves the same purpose, but for the Kubelet. Again, this has to be here when you first bootstrap the node; adding it afterward won't work.
* Because this is an HA cluster, `controlPlaneEndpoint` needs to point to the load balancer for your control plane nodes.
* The `clustername` value isn't technically necessary, but this allows you to specify individual clusters within a given set of AWS resources (make sure you use this name in the AWS tags on your resources).
* Technically, the `networking.dnsDomain` and `networking.serviceSubnet` aren't necessary either, but I'm including them here for completeness. `networking.podSubnet` is often required to be included for some CNI plugins (Calico, for example, requires this value to be set).
* You'll note there's no `etcd` section---this tells `kubeadm` to create an etcd node local to the control plane node, in what we call a "stacked control plane" configuration. If you're using an external etcd cluster, you'll need to add the appropriate configuration to this file.

Set the values you need in this file, then bootstrap your first control plane node with `kubeadm init --config=kubeadm.yaml --upload-certs` (substituting the correct filename, of course).

Once you've verified that the first control plane node is up, go ahead and install your CNI plugin.

### Adding More Control Plane Nodes

The 1.14 and 1.15 releases of Kubernetes added some significant functionality to `kubeadm` around the addition of control plane nodes. Thus, the addition of control plane nodes is _dramatically_ simpler now.

Here's a `kubeadm` configuration file you can use to join the second and third control plane nodes to the first node:

```yaml
---
apiVersion: kubeadm.k8s.io/v1beta2
kind: JoinConfiguration
discovery:
  bootstrapToken:
    token: 123456.a4v4ii39rupz51j3
    apiServerEndpoint: "cp-lb.us-west-2.elb.amazonaws.com:6443"
    caCertHashes: ["sha256:082feed98fb5fd2b497472fb7d9553414e27ff7eeb7b919c82ff3a08fdf5782f"]
nodeRegistration:
  name: ip-10-10-180-167.us-west-2.compute.internal
  kubeletExtraArgs:
    cloud-provider: aws
controlPlane:
  localAPIEndpoint:
    advertiseAddress: 10.10.180.167
  certificateKey: "f6fcb672782d6f0581a1060cf136820acde6736ef12562ddbdc4515d1315b518"
```

Some additional information on some of these values may be helpful:

* You'll be using `kubeadm join`, _not_ `kubeadm init`, so this needs to be a JoinConfiguration.
* If you don't know the value of a valid bootstrap token, you can create a new one with `kubeadm token create` and specify it for `discovery.bootstrapToken.token`.
* The `apiServerEndpoint` should be the load balancer for the control plane.
* If you don't know the SHA256 hash of the CA certificate, see [this post][xref-4].
* The value for `certificateKey` is only good for two hours, so you may need to use `kubeadm init phase upload-certs --upload-certs` on the initial control plane node if it has been more than two hours since the value was generated. This will generate a new certificate key value.
* As outlined in [this post][xref-3], the `controlPlane.localAPIEndpoint` value is what tells `kubeadm` this is a control plane node you're joining to the cluster.

Once you've set the values you need in this file, then just run `kubeadm join --config=kubeadm.yaml` (substituting the correct filename, of course). The node should join the cluster as a control plane node.

### Adding Worker Nodes

Because adding worker nodes uses `kubeadm join`, you'll find that the necessary `kubeadm` configuration here is similar to, but simpler, than the configuration for joining control plane nodes.

Here's a sample configuration file that would work (additional notes about this file are found below the example):

```yaml
---
apiVersion: kubeadm.k8s.io/v1beta2
kind: JoinConfiguration
discovery:
  bootstrapToken:
    token: 123456.a4v4ii39rupz51j3
    apiServerEndpoint: "cp-lb.us-west-2.elb.amazonaws.com:6443"
    caCertHashes: ["sha256:082feed98fb5fd2b497472fb7d9553414e27ff7eeb7b919c82ff3a08fdf5782f"]
nodeRegistration:
  name: ip-11-12-13-14.us-west-2.compute.internal
  kubeletExtraArgs:
    cloud-provider: aws
```

As noted earlier, you'll need a valid bootstrap token and the SHA256 hash of the CA certificate.

Once you have the correct values in this file, then run `kubeadm join --config=kubeadm.yaml` to join new worker nodes to the cluster (substitute the correct filename, of course).

## Wrapping Up

At this point, you should have a fully functional Kubernetes cluster running on AWS, with a functioning AWS cloud provider.

If you run into issues getting this to work correctly for you, I encourage you to pop over to the Kubernetes Slack instance (in the "#kubeadm" channel) and ask any questions. I tend to be there, as are a lot of the `kubeadm` contributors. You're also welcome to [contact me on Twitter][link-99] if you have any questions, comments, or corrections. (If you are contacting me for technical assistance, I do ask that you at least try some basic troubleshooting first. Thanks!)

Enjoy!

[link-1]: https://kubernetes.io/
[link-2]: https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/high-availability/
[link-3]: https://godoc.org/k8s.io/kubernetes/cmd/kubeadm/app/apis/kubeadm/v1beta2
[link-4]: https://github.com/kubernetes/cloud-provider-aws
[link-99]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2018-09-28-setting-up-the-kubernetes-aws-cloud-provider.md" >}}
[xref-2]: {{< relref "2019-02-18-kubernetes-kubeadm-and-the-aws-cloud-provider.md" >}}
[xref-3]: {{< relref "2019-04-16-using-kubeadm-add-new-control-plane-nodes-aws-integration.md" >}}
[xref-4]: {{< relref "2019-07-12-calculating-ca-certificate-hash-for-kubeadm.md" >}}
[xref-5]: {{< relref "" >}}
