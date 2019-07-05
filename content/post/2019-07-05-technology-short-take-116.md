---
author: slowe
categories: Information
comments: true
date: 2019-07-05T12:00:00Z
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- Automation
- MPLS
- Encryption
- SSL
- Kubernetes
- vSphere
- Azure
- BSD
- Certification
title: 'Technology Short Take 116'
url: /2019/07/05/technology-short-take-116/
---

Welcome to Technology Short Take #116! This one is a bit shorter than usual, due to holidays in the US and my life being busy. Nevertheless, I hope that I managed to capture something you find useful or helpful. As always, your feedback is welcome, so if you have suggestions, corrections, or comments, you're welcome to [contact me via Twitter][link-99].<!--more-->

## Networking

* David Gee [discusses][link-1] jSNAPy and how it can be used to enable unit tests in infrastructure-as-code scenarios.
* Jon Langemak [tackles][link-18] understanding RTs (Route Targets) and RDs (Route Distinguishers) are in MPLS VPNs. I also appreciate that Jon included a "Lab time" section in his article that encourages readers to try out the concepts he's explaining.

## Servers/Hardware

* Although I've by and large moved away from Apple hardware (I still have a MacBook Pro running macOS that sees very little use, and a Mac Pro running Fedora), I did see this article regarding [a new keyboard for the MacBook Air and MacBook Pro][link-17]. That's good---the butterfly keyboards are awful (in my opinion).

## Security

* If you're unfamiliar with public key infrastructure (PKI), digital certificates, or encryption, you may find [this Linux Journal article][link-3] helpful. It provides the basics behind X.509v3 digital certificates, how they help enable asymmetric (public/private key) encryption, and the connection to Transport Layer Security (TLS). Plus, there are some handy `openssl` commands! 
* As would be expected with any maturing open source project that is starting to see increased adoption, Kubernetes has seen its share of security vulnerabilities over the last couple of months. [This article][link-8] talks about a recent vulnerability in the `kubectl` command, which is typically used to interact with Kubernetes clusters.
* Lennart Koopmann provides a guide to [Yubikey authentication in the real world][link-10].

## Cloud Computing/Cloud Management

* Myles Gray takes an early look at the Kubernetes ClusterAPI provider for vSphere in [this post][link-6].
* Anthony Spiteri takes a few moments to talk about [his journey from working directly with the REST APIs of certain infrastructure products to using an infrastructure-as-code tool][link-9]. While each approach has its benefits and drawbacks, it's definitely true that most infrastructure-as-code tooling is far easier to approach than working with REST APIs directly.
* Audun Føyen describes how his company [settled on the use of `kustomize` for managing their Kubernetes manifests][link-11].
* [This blog post][link-13] outlines some of the changes/new features in `kubeadm` with the Kubernetes 1.15 release, including expanded support for streamlining the setup of HA control planes.
* David O'Brien has [a write-up on Azure Bastion][link-14], Microsoft's new "bastion-as-a-service" offering.

## Operating Systems/Applications

* There's been a fair amount of talk about "GitOps," particularly in the Kubernetes space, and [this recent article by Weaveworks][link-5] goes into a bit more detail about what GitOps is and what it seeks to achieve.
* Chris Humphries shares his experience in [using OpenBSD as his workstation][link-15].

## Storage

* Kubernetes 1.15 introduces alpha support for volume cloning. John Griffith of Red Hat provides more details in [this blog post on the Kubernetes web site][link-4]. There are some notable caveats for this alpha support (CSI drivers only, same storage class, same namespace), but all these are laid out in Griffith's article.
* Vito Botta provides [a few tips for OpenEBS][link-12].

## Virtualization

* Mischa Buijs reviews [using a SATADOM boot device with VMware ESXi][link-16].

## Career/Soft Skills

* Working effectively as a remote employee or as part of a distributed team is increasingly important. Via Chris Short, I saw [this CircleCI blog][link-2] talking about some best practices they've discovered/created for their distributed team. There's a few good ideas here that may be worth exploring in your situation or team as well.
* I liked David Varnum's article on [applying essentialism to certifications and skills development][link-7]. In other words, you can't know/learn everything, so be smart about where you choose to apply your time, focus, and attention.

That's all for now. Enjoy your weekend!

[link-1]: http://ipengineer.net/2019/04/iac-unit-tests-jsnapy-ansible/
[link-2]: https://circleci.com/blog/how-my-distributed-team-communicates-so-no-context-is-left-behind/
[link-3]: https://www.linuxjournal.com/content/understanding-public-key-infrastructure-and-x509-certificates
[link-4]: https://kubernetes.io/blog/2019/06/21/introducing-volume-cloning-alpha-for-kubernetes/
[link-5]: https://www.weave.works/blog/automate-kubernetes-with-gitops
[link-6]: https://blah.cloud/kubernetes/first-look-automated-k8s-lifecycle-with-clusterapi/
[link-7]: https://overlaid.net/2019/06/24/applying-essentialism-to-certifications-and-skills-development-in-the-tech-industry/
[link-8]: https://techerati.com/news-hub/new-kubernetes-flaw-discovered-command-line/
[link-9]: https://anthonyspiteri.net/infrastructure-as-code-vs-restful-apis-terraform-and-everything-in-between/
[link-10]: https://www.wtf.horse/2019/06/18/ubuntu-yubikey-authentication-in-the-real-world/
[link-11]: https://hackernoon.com/why-and-how-we-use-kustomize-for-kubernetes-deployment-843c942a4355
[link-12]: https://vitobotta.com/2019/07/03/openebs-tips/
[link-13]: https://kubernetes.io/blog/2019/06/24/automated-high-availability-in-kubeadm-v1.15-batteries-included-but-swappable/
[link-14]: https://david-obrien.net/2019/06/azure-bastion-rdp-ssh-access
[link-15]: https://sogubsys.com/openbsd-is-now-my-workstation-operating-system/
[link-16]: https://be-virtual.net/vmware-esxi-satadom-boot-device/
[link-17]: https://9to5mac.com/2019/07/04/kuo-new-keyboard-macbook-air-pro/
[link-18]: http://www.dasblinkenlichten.com/understanding-rts-and-rds/
[link-99]: https://twitter.com/scott_lowe
