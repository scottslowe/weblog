---
author: slowe
categories: Information
comments: true
date: 2018-02-12T12:00:00Z
tags:
- Kubernetes
- CLI
- AWS
- Azure
title: Some Tools to Help Learn Kubernetes
url: /2018/02/12/some-tools-help-learn-kubernetes/
---

[Kubernetes][link-1] is emerging as the clear leader in the container orchestration space. This makes it an important technology to know and understand. However, like other distributed systems, learning something like Kubernetes can be challenging due to the effort involved in getting Kubernetes up and running. It's not about learning to set up Kubernetes (although that comes in time); at first, it's about understanding what Kubernetes does and how to use Kubernetes. In this post, I'll share some tools to help learn what Kubernetes does and how to use Kubernetes.<!--more-->

Note that this post is **not** intended to be a comprehensive list of learning resources for Kubernetes. Also, this post is **not** focused on providing resources to help you learn to deploy Kubernetes. Instead, I'm focusing here on tools and services that let you get Kubernetes up and running quickly and easily so that you can focus on _using_ Kubernetes (deploying applications and workloads onto Kubernetes). I'm sure there are many more tools/options than what I have listed here; these are just some that I have used and feel might be useful for others.

I'll briefly cover the following tools and services:

* Minikube
* Kops
* Kube-aws
* Azure Container Service (ACS/AKS)

You'll note I'm focused on command-line tools here, since I'm a CLI junkie. Let's start with Minikube.

## Using Minikube

[Minikube][link-2] is a tool to run a single-node Kubernetes cluster locally on your system, using one of a number of supported hypervisors ([VirtualBox][link-3], [VMware Fusion][link-4], Hyper-V, xhyve, or KVM). Because it deploys a single-node Kubernetes cluster, you're limited in the types of things that you can do with the resulting Kubernetes cluster; however, it does offer the ability to work locally while most of the other tools require connectivity to a cloud provider (not unexpectedly, given the nature of what Kubernetes is and what it's designed to do). Of course, the trade-off for working locally is that your Kubernetes cluster won't be able to take advantage of cloud provider functionality, like services defined as type `LoadBalancer`.

(As an aside, since Minikube leverages `libmachine`---from Docker Machine---on the back-end, using both Minikube and Docker Machine with KVM on the same system has some interesting side effects. You've been warned.)

Despite the limitations, I think there's probably still some value in using Minikube for certain situations.

## Using Kops (Kubernetes Operations)

The [`kops` tool][link-5] can be used to (relatively) quickly and easily stand up Kubernetes clusters on [AWS][link-6] (support for other platforms is in the works). Typically, `kops` requires a DNS domain hosted in AWS Route 53 (although there are workarounds), and it does require an S3 bucket in which to store cluster/configuration state (no workaround for this). `kops` also allows you to create some pretty advanced configurations, such as HA deployments spread across multiple availability zones (AZs) and using SSH bastion hosts. I also like that you can use `kops` to generate [Terraform][link-7] configurations for setting up Kubernetes clusters.

However, you can also use it to set up quick and simple Kubernetes clusters for learning purposes. Here's an example command-line that could be used to turn up a small Kubernetes cluster using `kops`:

```
kops create cluster \
--node-count 3 \
--zones us-west-2a \
--dns-zone route53domain.com \
--node-size t2.large \
--networking flannel \
--ssh-public-key /path/to/public/key.pub \
clustername.route53domain.com
```

I won't go into all the details on using `kops` (though I might do another post on `kops` at some point in the future). Instead, I'll refer you to [the AWS guide in the `kops` repository][link-8] for more information and details on using `kops`. There are also a number of blog posts about there; [here's one example][link-9], and [here's one][link-12] from AWS themselves.

## Using kube-aws

As the name implies, [`kube-aws`][link-10] is a command-line tool for deploying a Kubernetes cluster on AWS. Like `kops`, `kube-aws` needs a Route 53 domain and an S3 bucket for storing state/configuration information. `kube-aws` also requires a KMS key. On the back-end, `kube-aws` is leveraging CloudFormation to spin up the Kubernetes cluster.

As with the other tools, I won't go into great detail on how to use `kube-aws`; instead, I'll direct you to [the documentation][link-11] (which seems really good).

## Using Azure Container Service (ACS/AKS)

Using the [Azure CLI][link-13], it's pretty easy to spin up a Kubernetes cluster. In fact, there are a couple of different ways to go about it.

You can use ACS (Azure Container Service) and specify Kubernetes as the orchestrator type (ACS supports other types of orchestrators as well):

    az acs create --orchestrator-type=kubernetes \
    --resource-group=my-grp --name=my-clus \
    --ssh-key-value=/path/to/public/key

After a 5-7 minutes, the command should complete and you'll be ready to roll with a Kubernetes cluster running in Azure. Use the `az acs kubernetes get-credentials` to configure `kubectl` to talk to the new cluster.

You can also use AKS (Azure Container Service, a newer offering that---as I understand it---supersedes/replaces ACS) to turn up a Kubernetes cluster (AKS only supports Kubernetes):

    az aks create --resource-group=my-grp --name=my-clus \
    --ssh-key-value /path/to/public/key

As with `az acs create`, after a few minutes you'll have a shiny new Kubernetes cluster up and running. From there, you can use `az aks upgrade` to potentially upgrade your cluster, or `az aks get-credentials` to pull down a configuration that allows `kubectl` to interact with your new cluster.

(Note that there was an issue with some versions of the Azure CLI prior to 2.0.25 that caused `az aks get-credentials` to fail. Upgrading to 2.0.25 or later seems to address the problem. If you need to upgrade the Azure CLI and you installed via `pip`, I recommend using the `--force-reinstall` flag to make sure the upgrade completes successfully.)

Sebastien Goasguen also has [a brief write-up of ACS/AKS][link-14] (he also covers Azure Container Instances, ACI).

## Why no mention of Google Kubernetes Engine (GKE)?

As a hosted/managed solution, GKE would fall into the same category as ACS/AKS. However, I didn't include GKE here for the simple reason that I was focusing on CLI-based tools, and (in my opinion) Google makes it _way_ too difficult to get the `gcloud` tool installed on your system. (Seriously---if you're going to write it in Python, why not just use `pip`?) There is a Docker container available, but at nearly 1GB in size (last time I checked) I don't feel that's a very attractive option either.

If you don't mind jumping through hoops to get `gcloud` installed (or if you'd prefer to use a GUI), then I'm sure GKE is a perfectly acceptable option.

There you have it...a few options for quickly and easily getting Kubernetes up and running (and you don't need a massive home lab, either). Enjoy!



[link-1]: https://kubernetes.io/
[link-2]: https://github.com/kubernetes/minikube
[link-3]: https://www.virtualbox.org/
[link-4]: https://www.vmware.com/products/fusion.html
[link-5]: https://github.com/kubernetes/kops
[link-6]: https://aws.amazon.com/
[link-7]: https://www.terraform.io/
[link-8]: https://github.com/kubernetes/kops/blob/master/docs/aws.md
[link-9]: https://ryaneschinger.com/blog/kubernetes-aws-vpc-kops-terraform/
[link-10]: https://github.com/kubernetes-incubator/kube-aws
[link-11]: https://kubernetes-incubator.github.io/kube-aws/
[link-12]: https://aws.amazon.com/blogs/opensource/running-bleeding-edge-kubernetes-on-aws-with-kops/
[link-13]: https://docs.microsoft.com/en-us/cli/azure/overview?view=azure-cli-latest
[link-14]: https://medium.com/bitnami-perspectives/az-aci-aks-acs-in-5-minutes-top-chrono-65c9952dfeb8
