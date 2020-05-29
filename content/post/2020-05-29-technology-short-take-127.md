---
author: slowe
categories: Information
comments: true
date: 2020-05-29T15:20:00-07:00
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- Linux
- Ubuntu
- VPN
- Kubernetes
- CAPI
- Docker
- KVM
- AWS
- Git
- NSX
- BGP
title: 'Technology Short Take 127'
url: /2020/05/29/technology-short-take-127/
---

Welcome to Technology Short Take #127! Let's see what I've managed to collect for you this time around...<!--more-->

## Networking

* Banzai Cloud's Toader Sebastian explains [how to write WASM (WebAssembly) filters for Envoy and deploy those filters with Istio][link-5]. Be forewarned---this is a bit of an advanced topic.
* Ivan Pepelnjak provides [a 101-level overview of AWS networking][link-21].
* This recent post by Eric Sloof shows [how to do BGP neighbor adjacency between GNS3 and NSX-T 3.0][link-30].
* Andrew Sy Kim tackles explaining a (somewhat) obscure part of Kubernetes networking: the `externalTrafficPolicy` setting. Read his write-up [here][link-33].

## Servers/Hardware

Nothing this time around, but I'll stay alert for items to include next time!

## Security

* Ubuntu 20.04 has backported support for WireGuard in its 5.4-based kernel, but for earlier versions some additional work is needed. Here's [a write-up on setting up WireGuard on Ubuntu 18.04][link-4].
* Bruce Schneier weighs in on the [security and privacy implications of Zoom][link-13]. (And while we're on the topic of Zoom, I'd love to hear your thoughts on their acquisition of Keybase.)
* Some security vulnerabilities with Thunderbolt were recently discovered and disclosed. See [this website][link-20] for more details.
* Looking for some security tools? [Look no further][link-24].

## Cloud Computing/Cloud Management

* Reader Witold Duranek contacted after [Technology Short Take 126][xref-1] to thank me for the useful information the series provides, and to mention that she (he?) has been [writing some stuff][link-1] during the Kubernetes learning process. Check it out---you may find something useful here.
* Jason Price shares [a fully fleshed-out example of Kubernetes Pod Security Policies][link-12].
* Ravi Jagannathan provides an overview of [using Velero to do backups and restores on Kubernetes][link-14].
* Michael Kashin [takes a stroll through Cluster API][link-16] using the Cluster API Provider for Docker (CAPD).
* Christian Posta of Solo.io [tackles the question][link-17] of whether an API gateway is needed if a service mesh is present. I haven't dug deep enough into this space (yet) to have an informed opinion, but from my uninformed perspective it seems that these two technologies are more complementary than competitive.
* Ben Kehoe [laments some of CloudFormation's shortcomings][link-18] in acting like a proper infrastructure graph management service.
* Kyle Galbraith takes readers through [the use of AWS Organizations to simplify billing for multiple accounts][link-19].
* Adin Ermie provides [a review of _Managing Kubernetes_][link-25], a book by Brendan Burns and my colleague Craig Tracey.
* Here are some [resources for learning Kubernetes][link-32].

## Operating Systems/Applications

* Nicola Apicella has a nice, in-depth write-up discussing [Docker images, overlay file systems, and the OCI specification][link-3].
* Kirill Shirinkin has a four-part "Dockerless" series going ([part 1][link-6], [part 2][link-7], [part 3][link-8], and [part 4][link-9]) that discusses replacing Docker with...well, something else.
* Mat Jovanovic shares the story of [how he moved from Blogger to Hugo on AWS][link-11].
* [This article][link-22] provides some information on how Bash startup files are used by the shell.
* Brian Vanderwal [writes about Git's worktree feature][link-23], which looks to be _really_ handy.
* I can't immediately think of a use case for [this configuration][link-29], but it's handy to know it's possible if necessary.

## Storage

I don't have anything to share this time, but maybe check [here][link-28] for something that strikes your interest?

## Virtualization

* For those new to Linux virtualization---or even if you've been around Linux virtualization for a while---this article explaining [how Intel VT-x, QEMU, and KVM work together][link-15] is full of good information.
* Tomas provides resources for [running Fedora Silverblue on Libvirt/KVM][link-31].

## Career/Soft Skills

* Former manager and colleague Paul Lundin penned [this piece][link-2] about working in a startup versus working in an enterprise back in June 2018. There are some very salient points here for folks trying to decide their career path.
* Matthias Ender has an amusing [collection of stories around the origins of various computer terms][link-10].
* [Here are some templates][link-26] for Wardley mapping. If you don't know what Wardley mapping is, see [here][link-27].

That's all for now! I hope I have included something that is useful to you. If you have feedback or suggestions for improvement, I'd love to hear from you (reaching [me on Twitter][link-99] is probably easiest). Thanks for reading!

[link-1]: http://blog.witalis.net/
[link-2]: https://medium.com/startup-grind/startup-or-enterprise-a-tale-of-two-careers-9b4acfe79577
[link-3]: https://dev.to/napicella/how-are-docker-images-built-a-look-into-the-linux-overlay-file-systems-and-the-oci-specification-175n
[link-4]: https://linuxize.com/post/how-to-set-up-wireguard-vpn-on-ubuntu-18-04/
[link-5]: https://banzaicloud.com/blog/envoy-wasm-filter/
[link-6]: https://mkdev.me/en/posts/dockerless-part-1-which-tools-to-replace-docker-with-and-why
[link-7]: https://mkdev.me/en/posts/dockerless-part-2-how-to-build-container-image-for-rails-application-without-docker-and-dockerfile
[link-8]: https://mkdev.me/en/posts/dockerless-part-3-moving-development-environment-to-containers-with-podman
[link-9]: https://mkdev.me/en/posts/the-tool-that-really-runs-your-containers-deep-dive-into-runc-and-oci-specifications
[link-10]: https://endler.dev/2020/folklore/
[link-11]: https://www.matscloud.com/blog/2020/04/24/hugo-with-docsy-and-aws-amplify/
[link-12]: https://developer.squareup.com/blog/kubernetes-pod-security-policies/
[link-13]: https://www.schneier.com/blog/archives/2020/04/security_and_pr_1.html
[link-14]: https://medium.com/@ravijagannathan/backup-and-dr-in-k8-f7e3f0fd2946
[link-15]: https://binarydebt.wordpress.com/2018/10/14/intel-virtualisation-how-vt-x-kvm-and-qemu-work-together/
[link-16]: https://networkop.co.uk/post/2020-05-cluster-api-intro/
[link-17]: https://blog.christianposta.com/microservices/do-i-need-an-api-gateway-if-i-have-a-service-mesh/
[link-18]: https://read.acloud.guru/cloudformation-is-an-infrastructure-graph-management-service-and-needs-to-act-more-like-it-fa234e567c82
[link-19]: https://blog.kylegalbraith.com/2018/11/20/simplify-your-aws-billing-for-multiple-accounts-using-organizations/
[link-20]: https://thunderspy.io/
[link-21]: https://blog.ipspace.net/2020/05/aws-networking-101.html
[link-22]: https://linuxize.com/post/bashrc-vs-bash-profile/
[link-23]: https://spin.atomicobject.com/2016/06/26/parallelize-development-git-worktrees/
[link-24]: https://tools.tldr.run/
[link-25]: https://adinermie.com/resources/technical-book-reviews/book-review-managing-kubernetes-operating-kubernetes-clusters-in-the-real-world/
[link-26]: https://blogs.endjin.com/2020/03/office365-wardley-mapping-templates/
[link-27]: https://medium.com/wardleymaps
[link-28]: https://jmetz.com/2020/04/storage-short-take-28/
[link-29]: https://linuxconfig.org/how-to-enable-multiple-simultaneous-audio-outputs-on-pulseaudio-in-linux
[link-30]: https://www.ntpro.nl/blog/archives/3574-BGP-Neighbor-Adjacency-between-GNS3-and-NSX-T-3.0.html
[link-31]: https://www.tomas.io/articles/silverblue
[link-32]: https://ramitsurana.github.io/awesome-kubernetes/
[link-33]: https://www.asykim.com/blog/deep-dive-into-kubernetes-external-traffic-policies
[link-99]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2020-04-24-technology-short-take-126.md" >}}
