---
author: slowe
categories: Information
comments: true
date: 2018-12-21T12:00:00Z
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- AWS
- Kubernetes
- NSX
- VMware
- macOS
- Azure
- Fusion
- Linux
- Windows
- Microsoft
title: 'Technology Short Take 108'
url: /2018/12/21/technology-short-take-108/
---

Welcome to Technology Short Take #108! This will be the last Technology Short Take of 2018, so here's hoping I can provide something useful for you. Enjoy!<!--more-->

## Networking

* Maish Saidel-Keesing has a 5-part series on replacing the AWS ELB. This is an older post (from August) that I've had in my backlog for a while, and I'm just now getting around to reading the series. There's some really good information in here. I won't link to all five, but rather just point you at [the introductory post][link-9] (and Maish has done a great job---better than a lot of bloggers---making the entire series easily accessible).
* Quentin Machu [delves deep into an obscure DNS resolution issue][link-10] that was introducing seemingly-erratic delays with DNS lookups. Quentin's post is very detailed, and has lots of good information.
* Anthony Burke has some information on [preparing a node for Kubernetes and the NSX NCP][link-16].
* Benjamin Dale suggests that [you don't know JunOS][link-25].

## Servers/Hardware

* On the hardware side of things, we have James Hamilton of AWS discussing two hardware-related items coming out of AWS re:Invent 2018. First, Hamilton [discusses the Arm-based AWS Graviton processor][link-1] powering the newly-announced A1 family of EC2 instances. Next, Hamilton [tackles the need for the Inferentia machine learning processor][link-2] that will help reduce the computing power required for ML workloads. Both posts are a bit high-level, but a useful read nevertheless.

## Security

* Kirill Kuznetsov has [a macOS-specific write-up on using Yubikeys with GPG and SSH][link-11].
* David Hepkin [discusses][link-13] the HyperClear mitigation for L1TF under Hyper-V.
* Kenneth Hui digs into [using AWS KMS Custom Key Store with CloudHSM to encrypt your data][link-20].

## Cloud Computing/Cloud Management

* Here's [another stab at a multi-cloud control plane][link-3].
* My colleague Josh Rosso has a post up on [securing communication to the Kubernetes controller manager and Kubernetes scheduler][link-4]. It even includes a video---nice!
* This is [a slightly older article by Mitch Beaumont][link-6] that I've had sitting in the hopper for a while. When I first read it, I was still (relatively) early in my Kubernetes journey, and a fair amount of what he discussed wasn't quite "clicking" in my brain (no fault of his). Now, it makes a lot more sense to me. Time and experience can make quite a difference sometimes!
* Weijuan Shi Davis collected some resources related to Windows containers on Kubernetes in [this post-KubeCon post][link-12].

## Operating Systems/Applications

* Suvash Thapaliya describes a mechanism for [building a self-documenting `Makefile`][link-5].
* In part 5 of a series on Kubernetes metrics, Bob Cotton discusses [etcd metrics][link-8]. (This post reminds me that I need to write up the procedure I followed to get the Prometheus Operator scraping metrics from a TLS-secured etcd cluster bootstrapped using `kubeadm`.)
* Kyle Ruddy helps folks [get started with the Desired State Configuration Resources (DSCR) for VMware][link-14], a new way of using PowerShell DSC with VMware vSphere environments.
* Nick Janetakis [describes][link-17] what he finds to be---in his words---"a really, really good terminal set up" for WSL.
* [This article by Paul Johnston][link-22] provided (for me, anyway) a very usable and understandable explanation of Lambda Layers and custom runtimes for Lambda.
* Mikhail Shilkov [provides a great overview][link-23] of Azure Durable Functions and how they fit into the bigger picture of microservices architectures, serverless platforms, and event-driven models.

## Storage

* Nilesh Jayanandana shares some information on [a case study for NFS with Kubernetes][link-7] (via GKE).
* Stephen Foskett explains [how to add a mirror to an existing ZFS drive][link-19].

## Virtualization

* William Lam provides [some resources to learning more about the AWS Nitro platform][link-15]. There are some good links here to video recordings of re:Invent sessions, so go install `youtube-dl` if you haven't already and download those videos for offline viewing!
* Dave Taylor provides s[ome tips for configuring VMware Fusion for Linux usage][link-24]. Personally, I say just switch to Linux full-time, but hey not everyone is down for that.

## Career/Soft Skills

* Via the AWS News blog, Michael Wittig shares some information on [how to become an AWS expert][link-18]. While the article is specific to AWS (a useful skill to have), the tips that he shares can be equally applicable in learning other technology areas as well.
* Emily Ludolph shares [some ideas for leaders][link-21]. While this originates in the "creative" industry, the ideas shared here are, I think, equally applicable in other industries.

That's it for the Technology Short Take series in 2018! Look for the next one in early 2019. Until then (or even after then), feel free [to hit me on Twitter][link-99] if you have any questions, comments, or suggestions for improvement. Have a great remainder of 2018, folks!

[link-1]: https://perspectives.mvdirona.com/2018/11/aws-designed-processor-graviton/
[link-2]: https://perspectives.mvdirona.com/2018/11/aws-inferentia-machine-learning-processor/
[link-3]: https://blog.upbound.io/introducing-crossplane-open-source-multicloud-control-plane/
[link-4]: https://joshrosso.com/posts/secure-port-k8s-cm-sched
[link-5]: https://suva.sh/posts/well-documented-makefiles/
[link-6]: http://www.mitchyb.com/2018/05/mid-week-fun-with-draft-kubernetes-and.html
[link-7]: https://medium.com/platformer-blog/nfs-persistent-volumes-with-kubernetes-a-case-study-ce1ed6e2c266
[link-8]: https://blog.freshtracks.io/a-deep-dive-into-kubernetes-metrics-part-5-etcd-metrics-6502693fa58
[link-9]: https://technodrone.blogspot.com/2018/08/replacing-aws-elb-problem.html
[link-10]: https://blog.quentin-machu.fr/2018/06/24/5-15s-dns-lookups-on-kubernetes/
[link-11]: https://evilmartians.com/chronicles/stick-with-security-yubikey-ssh-gnupg-macos
[link-12]: https://blogs.technet.microsoft.com/virtualization/2018/12/15/kubecon-windows-containers-on-kubernetes-and-101-materials-for-your-holiday-reading/
[link-13]: https://blogs.technet.microsoft.com/virtualization/2018/08/14/hyper-v-hyperclear/
[link-14]: https://blogs.vmware.com/PowerCLI/2018/12/getting-started-dsc-for-vmware.html
[link-15]: https://www.virtuallyghetto.com/2018/12/learning-more-about-the-nitro-platform-which-will-power-vmware-cloud-on-aws-outposts.html
[link-16]: https://networkinferno.net/preparing-a-node-for-kubernetes-and-nsx-ncp
[link-17]: https://nickjanetakis.com/blog/conemu-vs-hyper-vs-terminus-vs-mobaxterm-terminator-vs-ubuntu-wsl
[link-18]: https://aws.amazon.com/blogs/aws/how-to-become-an-aws-expert/
[link-19]: https://blog.fosketts.net/2017/12/11/add-mirror-existing-zfs-drive/
[link-20]: https://cloudarchitectmusings.com/2018/12/18/using-aws-kms-custom-key-store-with-cloudhsm-to-encrypt-your-data/
[link-21]: https://99u.adobe.com/articles/60545/from-checking-your-ego-to-making-meetings-less-scary-for-introverts-99us-10-best-ideas-for-leaders
[link-22]: https://medium.com/@PaulDJohnston/lambda-layers-and-custom-runtimes-cdb4d9a848dc
[link-23]: https://hackernoon.com/making-sense-of-azure-durable-functions-645ecb3c1d58
[link-24]: https://www.askdavetaylor.com/how-to-configure-vmware-fusion-for-linux-os-usage/
[link-25]: http://blog.eighthlayer.io/you-dont-know-junos/
[link-99]: https://twitter.com/scott_lowe
