---
author: slowe
categories: Information
comments: true
date: 2018-02-24T10:00:00Z
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- Cisco
- macOS
- Kubernetes
- Terraform
- VMware
- NSX
- AWS
- Azure
- vSphere
- Docker
- CLI
- Ubuntu
- VirtualBox
- KVM
title: 'Technology Short Take 95'
url: /2018/02/24/technology-short-take-95/
---

Welcome to Technology Short Take 95! This Short Take was a bit more challenging than normal to compile, given that I spent the week leading up to its publication visiting customers in Europe. (My travel schedule in Europe is also why it didn't get published until Saturday instead of the typical Friday.) Nevertheless, I have persevered in order to deliver you this list of links and articles. I hope it proves useful!<!--more-->

## Networking

* Larry Smith Jr. has [a nice write-up on Cisco XR][link-1] stemming from a presentation at NFD 17.
* VMware recently released a reference design guide for NSX-T; see [here][link-16] for more details.
* The engineering team at Lyft recently discussed a new overlay-free networking approach they've been working on for Kubernetes: [IPVLAN-based CNI stack for running within VPCs on AWS][link-17]. This is pretty cool, but does introduce some potential design considerations for deploying Kubernetes on AWS. (For those that may be unfamiliar: CNI, or Container Network Interface, is the means whereby network mechanisms "plug into" Kubernetes. IPVLAN is a low-latency means of providing IP connectivity to containers. VPCs, or Virtual Private Clouds, are Amazon's software-defined networking mechanism for workloads running on AWS.)
* Viktor van den Berg writes on [deploying NSX load balancers with vRA][link-18].
* Alen Komljen provides [an introductory overview][link-21] of service meshes in Kubernetes.
* Matt Klein has a great post on [why we need to embrace eventual consistency in (distributed) networking][link-24].
* In the event that you need yet-another-introduction to Kubernetes networking, Mark Betz has a three-part series you may find helpful/useful ([part 1][link-27], [part 2][link-28], and [part 3][link-29]).

## Servers/Hardware

Nothing this time around, sorry!

## Security

* [This][link-3] doesn't make me feel very secure.

## Cloud Computing/Cloud Management

* Dimitri de Swart has a write-up on [LAMP stacks made easy with VMware and Puppet][link-5].
* Fabrizio Pandini shares [a "protip" on using `kubeadm phases` when deploying Kubernetes clusters][link-7]. I must admit that I haven't yet played with `kubeadm`; guess that's moving up my list of "things to try."
* Sebastien Goasguen [shares a few `kubectl` tricks][link-8] in a discussion on imperative configuration versus declarative configuration.
* Here's another thing I really need to take out for a spin: [the Kubernetes provider for Terraform][link-9].
* Andrés Martínez, a software engineer with Bitnami, shares a write-up on [a serverless service mesh with kubeless and Istio][link-10]. (And he provides [a GitHub repository][link-11] with samples from the article.)
* Looks like PKS, the joint Kubernetes effort from VMware and Pivotal, has gone GA (see [this VMware blog post][link-14]) with initial support for vSphere and GCP. I think the omission of AWS support at GA is a mistake, but what do I know?
* Courtesy of Corey Quinn, here's [an awesome AWS versus Azure comparison][link-15], comic book style.
* Most folks have probably heard about 12 Factor Applications (see [here][link-19] if you haven't or if you need a refresher); [this article][link-20] talks about 12 basic rules and good practices for using Kubernetes optimally.
* If you're interested in building a local Kubernetes cluster on your laptop---and there may be various reasons why this would be helpful---[this guide by Mike Gavrilov on Kubernetes on Ubuntu on VirtualBox][link-26] could have some useful information for you.

## Operating Systems/Applications

* The Project Atomic blog shares [how to use OCI image registries with Buildah][link-6].
* Speaking of Buildah...here's a post from Tomáš Tomeček on [using Buildah and Ansible to create container images][link-30].
* Jesse Hu has shared [a Helm chart for deploying VMware's open source Harbor registry][link-12]. From what I can see, it looks like Jesse's version is a forked (and updated) version of [a similar Helm chart from Paul Czarkowski][link-13].
* Cormac Hogan has [a first look at AWS Greengrass on vSphere][link-22].
* [This][link-23] is a pretty interesting use case for Docker.

## Storage

* I had a chance to take a (very) quick look at [Dotmesh][link-2], a new solution that enables snapshots for stateful workloads in Kubernetes and Docker. It seems like an interesting and potentially helpful solution, although I haven't (yet) had the time to actually spend any hands-on time with it.

## Virtualization

* Alex Ellis provides [an overview of KVM][link-4], in the context of having another option for deploying your own Kubernetes cluster.
* Alessandro Arrichiello [discusses how to set up kubevirt][link-25], a project aimed at enabling Kubernetes to manage VMs (via libvirt).

## Career/Soft Skills

Nothing this time around (which surprises me---I almost _always_ find material to include in this section).

OK, that's all this time around, but I've already started gathering material for next time. Look for the next Technology Short Take in about two weeks. Thanks for reading!



[link-1]: https://everythingshouldbevirtual.com/bringing-devops-to-routing-cisco-xr/
[link-2]: https://dotmesh.com/
[link-3]: https://krausefx.com/blog/mac-privacy-sandboxed-mac-apps-can-take-screenshots
[link-4]: https://blog.alexellis.io/kvm-kubernetes-primer/
[link-5]: https://www.vmguru.com/2018/01/lamp-stacks-made-easy-with-vmware-and-puppet/
[link-6]: http://www.projectatomic.io/blog/2018/01/using-image-registries-with-buildah/
[link-7]: https://blog.heptio.com/how-to-start-creating-kubernetes-clusters-like-a-pro-kubeadm-phases-heptioprotip-2bdce58b530d
[link-8]: https://medium.com/bitnami-perspectives/imperative-declarative-and-a-few-kubectl-tricks-9d6deabdde
[link-9]: https://www.terraform.io/docs/providers/kubernetes/index.html
[link-10]: https://engineering.bitnami.com/articles/serverless-service-mesh-with-kubeless-and-istio.html
[link-11]: https://github.com/andresmgot/kubeless-istio-sample
[link-12]: https://github.com/jessehu/helm-harbor
[link-13]: https://github.com/paulczar/helm-harbor
[link-14]: https://blogs.vmware.com/cloudnative/2018/02/12/pivotal-container-service-ga/
[link-15]: https://www.simplilearn.com/aws-vs-azure-cloud-certification-article
[link-16]: https://blogs.vmware.com/networkvirtualization/2018/02/introducing-vmware-nsx-t-reference-design.html
[link-17]: https://eng.lyft.com/announcing-cni-ipvlan-vpc-k8s-ipvlan-overlay-free-kubernetes-networking-in-aws-95191201476e
[link-18]: https://www.viktorious.nl/2018/02/13/one-armed-versus-inline-load-balancer-with-vra-and-nsx/
[link-19]: https://12factor.net/
[link-20]: https://blog.octo.com/en/the-twelve-factors-kubernetes/
[link-21]: https://akomljen.com/kubernetes-service-mesh/
[link-22]: https://cormachogan.com/2018/02/13/first-look-aws-greengrass-vsphere/
[link-23]: http://www.vmwareinfo.com/2018/02/journey-to-docker.html
[link-24]: https://blog.envoyproxy.io/embracing-eventual-consistency-in-soa-networking-32a5ee5d443d
[link-25]: https://medium.com/@alezzandro/kubernetes-and-virtualization-kubevirt-will-let-you-spawn-virtual-machine-on-your-cluster-e809914cc783
[link-26]: https://itnext.io/kubernetes-on-ubuntu-on-virtualbox-60e8ce7c85ed
[link-27]: https://medium.com/google-cloud/understanding-kubernetes-networking-pods-7117dd28727
[link-28]: https://medium.com/google-cloud/understanding-kubernetes-networking-services-f0cb48e4cc82
[link-29]: https://medium.com/google-cloud/understanding-kubernetes-networking-ingress-1bc341c84078
[link-30]: https://blog.tomecek.net/post/building-containers-with-buildah-and-ansible/
