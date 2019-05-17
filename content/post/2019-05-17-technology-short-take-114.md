---
author: slowe
categories: Information
comments: true
date: 2019-05-17T12:00:00Z
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- Kubernetes
- Linux
- vSphere
- AWS
- Pulumi
- Terraform
- VDI
- Productivity
title: 'Technology Short Take 114'
url: /2019/05/17/technology-short-take-114/
---

Welcome to Technology Short Take #114! There will be a longer gap than usual before the next Tech Short Take (more details to come on Monday), but in the meantime here's some articles and links to feed your technical appetite. Enjoy!<!--more-->

## Networking

* Courtesy of Tigera, Alex Pollitt shares [some guidelines on when Linux `conntrack` is no longer your friend][link-9].

## Servers/Hardware

* Apparently Dell's new docking stations [support firmware updates via Linux][link-13]. Nice!

## Security

* Michael Allen published an article on [weaponizing the Yubikey][link-6]. It's an interesting read, although I'm not sure about the usefulness or practicality of the technique he describes.
* This is [a pretty detailed write-up of a remote code execution vulnerability prsent on most Dell computers][link-7] (courtesy of Dell SupportAssist). I'm thankful that Bill Demirkapi followed a responsible disclosure policy.
* Here's a summary of [attacks against GPG-signed APT repositories][link-11]. The "TL;DR" on this one is simple: _always serve your APT repositories over TLS._
* Since we're on a bit of a security kick this time around, then [the recent announcement by HyTrust of HyTrust CloudControl 6.0][link-12] seems appropriate to include. There's some interesting new functionality found in this release, including support for/integration with Kubernetes.

## Cloud Computing/Cloud Management

* [This article][link-2] by Bob Killen provides a good foundation of information on understanding Kubernetes authentication (AuthN) and authorization (AuthZ; implemented via RBAC).
* For folks that might be a bit newer to the Kubernetes community, [this InfoQ article][link-3] provides a fairly thorough introduction to Kubernetes concepts and building blocks. More advanced users won't find much useful, so if you've already mastered some of the basics I wouldn't bother spending time reading it. This isn't a knock against the article, just a frank observation about its usefulness to varying experience levels.
* I included this link on [scaling Kubernetes to 2,500 nodes][link-4] in [TST 94][xref-1], but wanted to include it again here. If you are like me, your knowledge of Kubernetes has grown progressively since last February, and some of the recommendations made here may make more sense now in the context of the additional knowledge you've acquired.
* Andy Chou [introduces faast.js][link-8], a new project to make serverless functions super-easy to use.
* Cody De Arkland spent some time with Pulumi and has an article on [setting up a VPN between his home lab and AWS using Pulumi][link-16].
* Speaking of Pulumi, Kyle Galbraith wrote up [a comparison of Pulumi and Terraform for infrastructure as code][link-18].
* Richard Bejarano [rants a bit about container misconceptions][link-20].
* The CNCF blog has a great article written by an Alibaba software engineer (Xingyu Chen) on [some performance optimizations][link-21] for etcd that have been contributed back to the open source community.

## Operating Systems/Applications

* This article has good information on [safely using `/tmp` and `/var/tmp`][link-1] on systemd-powered Linux distributions.
* Cindy Sridharan has a good article on [health checks and graceful degradation in distributed systems][link-5]. This is a slightly older article (August 2018), but an informative one (to me, at least). The article explains a few orchestration concepts (like liveness probes and readiness probes in Kubernetes), but mostly focuses on the need for fine-grained health checks in applications.
* Microsoft recently [introduced][link-10] some new remote development extensions for Visual Studio Code. These look interesting, but be aware that they are not licensed with an open source license. Additionally, the wording of the license for these extensions has caused some consternation (at least, among the folks I follow on Twitter). I'll leave it to readers to come to their own conclusions.
* Sinny Kumari shares some information on [running Fedora CoreOS on libvirt][link-22].

## Storage

* Cormac Hogan walks through [using Portworx and STORK on vSphere for container volume snapshots][link-19].

## Virtualization

* I don't normally post a lot of EUC (end-user computing) content, but this time around I have two articles that popped onto my radar. First, there's Johan Van Amersfoort who discusses [building Linux VDI for deep learning workloads][link-14] (looks like this may be the first in a series, so if this is of interest to you be sure to keep an eye on his site). The second article is by Rob Beekmans examining [several virtual desktop platforms][link-15] available to customers/users.

## Career/Soft Skills

* Sometimes I'll include productivity-related stuff in this section. Here's an article I found recently on [a variation of the Pomodoro technique called FlowTime][link-17]. 

OK, that's all for now. As always, feel free to provide any feedback [via Twitter][link-99]. Have a great weekend!

[link-1]: https://systemd.io/TEMPORARY_DIRECTORIES.html
[link-2]: https://medium.com/@mrbobbytables/kubernetes-day-2-operations-authn-authz-with-oidc-and-a-little-help-from-keycloak-de4ea1bdbbe
[link-3]: https://www.infoq.com/articles/kubernetes-effect
[link-4]: https://openai.com/blog/scaling-kubernetes-to-2500-nodes/
[link-5]: https://medium.com/@copyconstruct/health-checks-in-distributed-systems-aa8a0e8c1672
[link-6]: https://www.blackhillsinfosec.com/how-to-weaponize-the-yubikey/
[link-7]: https://d4stiny.github.io/Remote-Code-Execution-on-most-Dell-computers/
[link-8]: https://faastjs.org/blog/2019/05/02/introducting-faastjs
[link-9]: https://www.tigera.io/blog/when-linux-conntrack-is-no-longer-your-friend/
[link-10]: https://code.visualstudio.com/blogs/2019/05/02/remote-development
[link-11]: https://blog.packagecloud.io/eng/2018/02/21/attacks-against-secure-apt-repositories/
[link-12]: https://www.hytrust.com/news-item/hytrust-offers-free-container-security-in-expanded-security-solution-for-hybrid-clouds/
[link-13]: https://blogs.gnome.org/hughsie/2019/05/02/updating-the-firmware-on-new-dell-docks/
[link-14]: https://vhojan.nl/mastering-voodoo-building-linux-vdi-for-deep-learning-workloads-part-1/
[link-15]: https://robbeekmans.net/euc/desktops-platforms-to-deploy-on-the-choice-is-yours/
[link-16]: https://www.thehumblelab.com/homelab-vpn-and-aws-with-pulumi/
[link-17]: https://zapier.com/blog/flowtime-technique/
[link-18]: https://blog.kylegalbraith.com/2018/12/21/how-pulumi-compares-to-terraform-for-infrastructure-as-code/
[link-19]: https://cormachogan.com/2019/05/09/portworx-stork-and-container-volume-snapshots/
[link-20]: https://blog.bejarano.io/container-misconceptions.html
[link-21]: https://www.cncf.io/blog/2019/05/09/performance-optimization-of-etcd-in-web-scale-data-scenario/
[link-22]: https://sinny.io/2019/05/13/running-fedora-coreos-nightly-iso-and-qcow2-images-in-libvirt/
[link-99]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2018-02-09-technology-short-take-94.md" >}}
