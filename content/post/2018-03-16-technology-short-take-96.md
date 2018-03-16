---
author: slowe
categories: Information
comments: true
date: 2018-03-16T12:00:00Z
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- Kubernetes
- AWS
- Docker
- Git
- Azure
- Linux
- PowerCLI
- VirtualBox
- Dell
- VMware
- vSphere
title: 'Technology Short Take 96'
url: /2018/03/16/technology-short-take-96/
---

Welcome to Technology Short Take 96! Ahead, lying in wait, is a unique collection of links, articles, and thoughts about various data center technologies. Browse if you dare...OK, so I'm being a bit melodramatic. It's still some good stuff here!<!--more-->

## Networking

* Via Matt Oswalt and Michael Bushong, I came across [this article on Juniper's use of P4][link-9]. Interesting stuff...P4 definitely has the potential to dramatically reshape networking in new ways, in my humble opinion.
* Maxime Lagresle of XING outlines how they went about troubleshooting [an unexplained connection timeout on Kubernetes/Docker][link-11].
* Ajay Chenampara [outlines][link-16] how POAP (Power On Auto Provisioning), a feature of Cisco NX-OS, works to streamline provisioning new network switches.
* Don Schenck has [a high-level overview][link-24] of Istio and service meshes.
* Daniel Álvarez has a good article describing some [OVN profiling and optimizing][link-25] he recently performed. I believe the patches he mentioned in the post have already been accepted into the OVN codebase.

## Servers/Hardware

Nothing this time around; sorry! If you have some articles you feel are worthy of inclusion in the next Tech Short Take, send them my way!

## Security

* Joe Beda talks about [securing the Kubernetes dashboard][link-14] in the wake of several "cryptojacking" instances.
* CloudFlare discusses [recent amplification attacks][link-18] they're seeing. Perhaps these are tied to the major GitHub DDoS attack that happened recently?

## Cloud Computing/Cloud Management

* Daz Wilkin writes about [using Jsonnet to create a 10-domain Kubernetes ingress][link-1], then follows that up with a post on [using Google Container Builder to create a Jsonnet builder][link-2]. Neat stuff.
* Ross Kukulinski has a list of the 10 most common reasons Kubernetes Deployments fail, split across two parts ([part 1][link-7] and [part 2][link-8]). By the way, the use of capitalized "Deployments" is important---he's talking about the Kubernetes Deployments object, not the generalized idea of deploying Kubernetes.
* You've heard of blue/green deployments, but have you heard of "rainbow deployments"? Read [this post by Brian Dimcheff][link-10] to learn all about them. (Hint: the first six characters of a Git SHA hash are also a valid hex color.)
* Microsoft Azure Container Service (AKS) [now supports burstable VM types][link-12]. I've actually found myself using AKS a fair amount over the last couple of weeks.
* Dave Cheney has a great technical write-up on [using Contour with Let's Encrypt to secure web applications running on Kubernetes][link-13].
* John Jardin shares [his thoughts on Kubernetes on Azure][link-17].

## Operating Systems/Applications

* Adrian Hornsby has a series (two parts so far) on designing a multi-region active-active architecture. The focus of this series seems to be more on application architecture than infrastructure architecture, although there are clear overlaps in areas. Part 1 is [here][link-3]; part 2 is [here][link-4]. I highly recommend this series, with one caveat: it does occasionally feel like an AWS infomercial. Granted, Adrian works for AWS, so some of that is to be expected, but the Kool-Aid is a bit strong at times.
* Kevin Carter explains [how to ship journals remotely using `systemd-journald`][link-5].
* VMware PowerCLI 10.0 is [now available][link-15].
* I think I mentioned this before, but here's [an update on Mitogen][link-19]. Some very interesting possibilities ahead!

## Storage

* Chris Evans [discusses][link-20] a recent re-org that aligns ScaleIO with hardware sales. So much for software-defined storage?
* Erik Smith [explores][link-21] the Discovery problem with NVMe over Fabrics and how that problem might be resolved.

## Virtualization

* Ajeet Singh Raina talks about [running LinuxKit locally using VirtualBox][link-6].
* William Lam shares some information on [Thunderbolt-to-10 GbE adapters for ESXi][link-22].

## Career/Soft Skills

* Dislike sending out meeting invitations, or interested in being more effective with your meetings? Check out [Daniel Beck's post on better meeting invitations][link-23] (which will, in turn, lead to better meetings).

That's all for now! I'll have another Tech Short Take in a couple weeks. Thanks for reading!

[link-1]: https://medium.com/google-cloud/kubernetes-10-domain-ingress-c6c2bbf381bb
[link-2]: https://medium.com/@DazWilkin/a-jsonnet-builder-for-container-builder-71a0f6c18db7
[link-3]: https://hackernoon.com/the-quest-for-availability-771fa8a94a7c
[link-4]: https://medium.com/@adhorn/why-and-how-do-we-build-a-multi-region-active-active-architecture-6d81acb7d208
[link-5]: https://cloudnull.io/2018/02/journaling-remotely/
[link-6]: http://collabnix.com/running-linuxkit-locally-on-oracle-virtualbox-platform-made-easy/
[link-7]: https://kukulinski.com/10-most-common-reasons-kubernetes-deployments-fail-part-1/
[link-8]: https://kukulinski.com/10-most-common-reasons-kubernetes-deployments-fail-part-2/
[link-9]: https://forums.juniper.net/t5/The-New-Network/Juniper-Advancing-Disaggregation-Through-P4-Runtime-Integration/ba-p/319195
[link-10]: http://brandon.dimcheff.com/2018/02/rainbow-deploys-with-kubernetes/
[link-11]: https://tech.xing.com/a-reason-for-unexplained-connection-timeouts-on-kubernetes-docker-abd041cf7e02
[link-12]: https://azure.microsoft.com/en-us/blog/introducing-burstable-vm-support-in-aks/
[link-13]: https://blog.heptio.com/how-to-deploy-web-applications-on-kubernetes-with-heptio-contour-and-lets-encrypt-d58efbad9f56
[link-14]: https://blog.heptio.com/on-securing-the-kubernetes-dashboard-16b09b1b7aca
[link-15]: https://blogs.vmware.com/PowerCLI/2018/02/powercli-10.html
[link-16]: https://blogs.cisco.com/developer/with-poap-youll-never-console-into-your-nexus-switches-again
[link-17]: http://bleedingcode.com/thoughts-microsoft-azure-kubernetes/
[link-18]: https://blog.cloudflare.com/memcrashed-major-amplification-attacks-from-port-11211/
[link-19]: http://pythonsweetness.tumblr.com/post/171589071872/quadrupling-ansible-performance-with-mitogen
[link-20]: https://blog.architecting.it/scaleio-becomes-software-defined-on-hardware/
[link-21]: http://brasstacksblog.typepad.com/brass-tacks/2017/12/nvme-over-fabrics-discovery-problem.html
[link-22]: https://www.virtuallyghetto.com/2018/03/thunderbolt-to-10gbe-network-adapters-for-esxi.html
[link-23]: https://ddbeck.com/better-meeting-invitations/
[link-24]: https://developers.redhat.com/blog/2018/03/06/introduction-istio-makes-mesh-things/
[link-25]: http://dani.foroselectronica.es/ovn-profiling-and-optimizing-ports-creation-434/
