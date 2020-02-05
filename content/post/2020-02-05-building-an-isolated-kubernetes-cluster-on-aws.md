---
author: slowe
categories: Tutorial
comments: true
date: 2020-02-05T13:15:00-08:00
tags:
- AWS
- Kubernetes
- Security
- Docker
title: Building an Isolated Kubernetes Cluster on AWS
url: /2020/02/05/building-an-isolated-kubernetes-cluster-on-aws/
---

In this post, I'm going to explore what's required in order to build an isolated---or Internet-restricted---[Kubernetes][link-1] cluster on AWS with full AWS cloud provider integration. Here the term "isolated" means "no Internet access." I initially was using the term "air-gapped," but these aren't technically air-gapped so I thought isolated (or Internet-restricted) may be a better descriptor. Either way, the intent of this post is to help guide readers through the process of setting up a Kubernetes cluster on AWS---with full AWS cloud provider integration---using systems that have no Internet access.<!--more-->

At a high-level, the process looks something like this:

1. Build preconfigured AMIs that you'll use for the instances running Kubernetes.
2. Stand up your AWS infrastructure, including necessary VPC endpoints for AWS services.
3. Preload any additional container images, if needed.
4. Bootstrap your cluster using `kubeadm`.

It's important to note that this guide does **not** replace my earlier post on [setting up an AWS-integrated Kubernetes cluster on AWS][xref-1] (written for 1.15, but valid for 1.16 and 1.17). All the requirements in that post still apply here. If you haven't read that post or aren't familiar with the requirements for setting up a Kubernetes cluster with the AWS cloud provider, I strongly recommend you go read the linked article. As I walk through the four steps I outlined above, I'll be supplementing the information provided in that blog post.

## Build Preconfigured AMIs

Without getting into how the acronym is pronounced (OK, fine, I'm on the three syllable side), the first step in building an isolated Kubernetes cluster is creating preconfigured AMIs that have all the necessary components already installed. It may seem obvious, but once you launch an instance based on the AMI, you won't able to install additional packages---because you won't have Internet access. Hence, your images must have everything they could need already done:

* All necessary packages and dependencies need to be installed
* All required persistent configuration changes (like disabling swap or enabling IP routing) needs to be done
* All the core Kubernetes container images need to be preloaded/already pulled

There are any number of ways to accomplish these things; the means that you take isn't really important. Two examples you can use as inspiration or possibly as a source for your own fork are [Wardroom][link-2] and [the Kubernetes "image-builder" repository][link-3] (specifically, the section of the repository focused on building Cluster API [CAPI] images). Both Wardroom and the CAPI portion of the "image-builder" repository are based on [Packer][link-4] and [Ansible][link-5]. For my testing, I used AMIs built using the Kubernetes "image-builder" repository.

Once your AMIs have been created, then you're ready to move to the next step: standing up the necessary AWS infrastructure.

## Stand up AWS Infrastructure

I won't go into too much detail here; this is already well-covered in other posts (see [the August 2019 article][xref-1] or my [original September 2018 article][xref-3] on setting up the AWS cloud provider). I will add a few salient points that are specific to setting up an isolated Kubernetes cluster:

* All of the Kubernetes nodes will land on private subnets, for which the setting to automatically assign a public IP address is turned off (disabled).
* You'll need at least one public subnet (for a bastion host, or whatever you're going to use to gain access to the private subnets).
* Your public subnet will need an Internet gateway or NAT gateway for Internet access. The private subnets won't need anything, naturally.
* If you want to be able to use an Internet-facing ELB to expose Kubernetes Services running on the Internet-restricted cluster (using a Service of type LoadBalancer, for example), you'll want to be sure that a public subnet exists in each availability zone (AZ) where a private subnet exists that might contain Kubernetes nodes.
* The private subnets won't have Internet access, but the AWS cloud provider needs to make a call to the EC2 and Elastic Load Balancing (ELB) API endpoints. By default, these are URLs that resolve to a _public IP address._ The instances won't be able to connect to the public IP addresses associated with these endpoints, and thus the AWS cloud provider will fail. To fix this, you need two VPC endpoints in the VPC where you are building the isolated cluster. One of the endpoints is for the EC2 service, and the second is for the ELB service. Both of these will be for the region where you're building the isolated Kubernetes cluster, and both of these VPC endpoints need to have private DNS enabled. (I wrote about how to create a VPC endpoint programmatically using Pulumi in [this post][xref-4], in case you want to tackle this with an infrastructure-as-code approach.) Note that the AWS cloud provider's Elastic Block Store (EBS) integration uses the EC2 APIs.
* The AWS resources need to have the correct tags assigned for the AWS cloud provider to work. This is explained in more detail in the linked posts above, as are the IAM role and policy requirements for the AWS cloud provider to work.

You'll want to be sure that the VPC endpoints are properly registered with private DNS entries before you proceed. Use `dig` (or whatever DNS lookup tool may be installed on your instances) to verify that the instances can resolve `ec2.<region>.amazonaws.com` and `elasticloadbalancing.<region>.amazonaws.com`, receiving back IP addresses that are part of the private subnets where your instances reside.

## Preload Additional Container Images

Out of all the various tools I've seen for building preconfigured images---to be fair, I'm sure I haven't seen all of them, or even a significant fraction of them---none of them handle preloading/pulling container images _other_ than those required by Kubernetes. In particular, none of them address the additional container images needed by your CNI plugin.

This is not a fault of these tools. The vast majority of the time, users are a) building clusters that have Internet access, and therefore it isn't necessary to preload the CNI plugin's container images; and b) the choice of CNI plugin is generally a "run-time" decision, not a "build-time" decision, meaning that most of the time users won't/don't know in advance when building a preconfigured image which CNI plugin they will use.

In this case, though, you _have_ to know which CNI plugin you're using because you can't simply rely on Kubernetes to pull whatever images are needed (it won't work since there's no Internet access). So you have two options:

1. Modify whatever tool you're using to build your preconfigured images to _also_ preload the container images needed by your CNI plugin of choice.
2. Manually load the container images on instances by using a bastion host or VPN from a system that does have Internet access. (It's pretty likely you'll have either an SSH bastion host or a VPN into the private subnets, since that will be the most expedient way to gain access to the instances running on those private subnets.)

For the second option, I recently [documented the process for preloading container images][xref-2] for use by Kubernetes using the CLI on [containerd][link-6]-based instances (the Kubernetes "image-builder" project creates machine images running containerd). As noted in that article, you can use Docker on your personal laptop (or some other system) to do `docker pull` and then `docker save` to get the images, then use `ctr` and `crictl` on the back-end Kubernetes nodes to import the images.

Regardless of which option you take---preloading the container images into the preconfigured machine images, or manually loading them in via some other system with connectivity---you'll want to be sure _all_ necessary container images are present on the instances before proceeding to the next step. For containerd-based systems, use `crictl images` to show all the container images that Kubernetes will be able to see when it is bootstrapped.

The Kubernetes-related images that should be present when you run this command are:

```
k8s.gcr.io/coredns
k8s.gcr.io/etcd
k8s.gcr.io/kube-apiserver
k8s.gcr.io/kube-controller-manager
k8s.gcr.io/kube-proxy
k8s.gcr.io/kube-scheduler
k8s.gcr.io/pause
```

You'll also need to see listed some container images for your CNI plugin. For recent versions of Calico, for example, you should see:

```
calico/cni
calico/kube-controllers
calico/node
calico/pod2daemon-flexvol
```

Once you've verified that both the Kubernetes-related container images _and_ the container images required for your CNI plugin are present, then you're ready to proceed.

## Bootstrap the Cluster

You're now ready to bootstrap your isolated Kubernetes cluster on AWS. You can use the `kubeadm` configuration files in [the August 2019 article][xref-1] for your nodes as a basis/starting point.

Note that in this particular case, I haven't yet tested HA configurations for isolated Kubernetes clusters, so you'd want to remove the `controlPlaneEndpoint` line from the configuration file for your control plane node. That would make the `kubeadm` configuration file for the control plane node (there will be only one) look something like this:

```yaml
---
apiVersion: kubeadm.k8s.io/v1beta2
kind: ClusterConfiguration
apiServer:
  extraArgs:
    cloud-provider: aws
clusterName: blogsample
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

When adding worker nodes, change the `discovery.bootstrapToken.apiServerEndpoint` to point to the DNS name for the control plane node, making their `kubeadm` configuration file look something like this:

```yaml
---
apiVersion: kubeadm.k8s.io/v1beta2
kind: JoinConfiguration
discovery:
  bootstrapToken:
    token: 123456.a4v4ii39rupz51j3
    apiServerEndpoint: "ip-1-2-3-4.us-west-2.compute.internal:6443"
    caCertHashes: ["sha256:082feed98fb5fd2b497472fb7d9553414e27ff7eeb7b919c82ff3a08fdf5782f"]
nodeRegistration:
  name: ip-11-12-13-14.us-west-2.compute.internal
  kubeletExtraArgs:
    cloud-provider: aws
```

Obviously, the sample configuration file above would need to have a valid bootstrap token and valid CA certificate hash, as output by `kubeadm init` on the control plane node. If you don't know these values, [this blog post][xref-5] explains how to get them.

Within a few minutes, you should have a non-HA (single control plane node) Kubernetes cluster up and running, with full AWS cloud provider integration. (Note you'll need to define a StorageClass for dynamic volume provisioning to work; this isn't done by default).

## How I Tested

I tested this using [Pulumi][link-7] to create all necessary AWS resources, including VPC, subnets, Internet gateway (for the public subnets), VPC endpoints, security groups, and instances. I used AMIs built using the Kubernetes "image-builder" repository, and used an SSH bastion to gain access to the private instances. I preloaded the container images needed by the Calico CNI plugin manually (not baked into the image), and then used `kubeadm` to bootstrap the cluster. The operating system on the instances was Ubuntu 18.04.

## Wrapping it up

Hopefully this walk-through is helpful in describing some of the challenges of setting up an isolated Kubernetes cluster on AWS. While this procedure lets you get an isolated cluster up and running, there are a whole host of considerations for _operating_ an isolated cluster:

* How are you going to get container images into the cluster? (I have a simple idea about how to handle this, thanks to a conversation with my colleague Duffie Cooley.)
* What applications are going to run on the cluster, and how will users (or other applications) access them?
* Kubernetes Services of type LoadBalancer will spawn an ELB, but by default those ELBs will be externally accessible. Is that what's intended? (If not, you'll want to be sure the `service.beta.kubernetes.io/aws-load-balancer-internal` annotation is present in your Service manifest.)

I'd strongly encourage readers to be sure to think through these questions---among others---before proceeding, and make sure that this design fits your specific use case.

If anyone has any questions, feel free to hit me up on [the Kubernetes Slack instance][link-8]; I can be found hanging out in the `#kubeadm` and `#kubernetes-users` channels, among others. (Sign up [here][link-9] if you're not already a registered user of the Kubernetes Slack instance.) Alternately, feel free [to contact me via Twitter][link-10]. I'd love to hear any feedback, suggestions for improvement, or corrections. Thanks!

[link-1]: https://kubernetes.io/
[link-2]: https://github.com/heptiolabs/wardroom
[link-3]: https://github.com/kubernetes-sigs/image-builder
[link-4]: https://packer.io/
[link-5]: https://www.ansible.com/
[link-6]: https://containerd.io/
[link-7]: https://www.pulumi.com/
[link-8]: https://kubernetes.slack.com/
[link-9]: https://slack.k8s.io/
[link-10]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2019-08-14-setting-up-aws-integrated-kubernetes-115-cluster-kubeadm.md" >}}
[xref-2]: {{< relref "2020-01-25-manually-loading-container-images-with-containerd.md" >}}
[xref-3]: {{< relref "2018-09-28-setting-up-the-kubernetes-aws-cloud-provider.md" >}}
[xref-4]: {{< relref "2020-01-25-creating-aws-vpc-endpoint-with-pulumi.md" >}}
[xref-5]: {{< relref "2019-08-15-reconstructing-the-join-command-for-kubeadm.md" >}}
