---
author: slowe
categories: Information
comments: true
date: 2018-02-09T12:00:00Z
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- NSX
- VMware
- Python
- Automation
- Docker
- Kubernetes
- vSAN
- Terraform
- AWS
- Kubernetes
- Ansible
- Microsoft
- Windows
title: 'Technology Short Take 94'
url: /2018/02/09/technology-short-take-94/
---

Welcome to Technology Short Take 94! Ready for another round of links, articles, and thoughts on data center technologies? (Who knows, maybe I'll throw a rant or two in there.) OK, enough rambling...here's the good stuff!<!--more-->

## Networking

* Amit Aneja has a two-part series (so far) explaining the routing architecture in NSX-T (which brings multi-hypervisor and multi-cloud support to the NSX platform). This is some good content and reminds me of the the old NVP/NSX content I generated back in the day. Ah, good times...anyway, check out Amit's stuff [here][link-1] and [here][link-2].
* Sam McGeown has [a nice diagram][link-5] of the communications channels between the various VMware NSX components.
* Roie Ben Haim has a post providing [an introduction to NSX and Kubernetes][link-14].
* Matt Oswalt [tackles][link-18] the idea of "intent-driven" or "intent-based" networking---all the rage right now---and outlines how something like this _must_ interact with domains outside of networking in order to be effective. I particularly liked his (mini-)rant about how network automation can't be only about making the network engineer's life easier. Oh, snap!
* I'm not really sure if this belongs in networking or not (how does one classify OS kernel-level work on networking and security?), but we'll stick it here anyway: check out [this blog post][link-20] on `cilium-health`, a tool for troubleshooting cluster connectivity.
* Here's an interesting post on how Simon Metzger [pulled together Salt, NAPALM, and Kubernetes][link-21] for a proof-of-concept on using Salt (via NAPALM) to manage network devices.
* It's good to see my friend Brent Salisbury blogging again, this time writing about [measuring bandwith using `iperf` and Docker][link-25].

## Servers/Hardware

Nothing this time around (sorry!). I'll stay alert for content that I might be able to include in the next Tech Short Take.

## Security

* If you're using CloudFormation to manage your AWS security groups in an infrastructure-as-code approach, you owe it to yourself to check out [this article][link-19] by Jose Luis Ordiales.
* Security is a many-faceted area, and one facet is appropriately protecting credentials. [This article][link-22] by Sjors Robroek shows one approach to appropriately protecting credentials when using automation tools.

## Cloud Computing/Cloud Management

* I guess we can talk automation in this section, right? Yasen Simeonov [talks about][link-3] the work that went into the NSX-T APIs (JSON-based, hooray!) and how to use the Python SDK for NSX-T. The content is a bit high-level, but given the medium (a blog post) it's hard to get super-deep. It serves as a decent introduction, at least.
* In the last Tech Short Take, I shared an article by Kynan Riley on node affinity. This time around, he's [talking about pod affinity][link-8].
* I'm a 1Password user (just wish they'd release a "real" Linux client and bring back support for local vaults to all platforms), so seeing [this article][link-16] on how Agile Bits (the company behind 1Password) is using Terraform with AWS was pretty cool.
* Kim Bottu walks readers through [using Ravello to rebuild a (home) lab][link-17] in about 20 minutes.
* Christopher Berner has a detailed post on [the lessons learned by scaling Kubernetes to 2,500 nodes][link-23].

## Operating Systems/Applications

* Michael Wittig [shares some details][link-4] on migrating to Amazon Linux 2.
* Over the last couple of weeks I came across two articles related to creating Docker images. The first comes from Shubham Sharma, which provides [some tips on writing a Dockerfile][link-6]. The second comes from Matthew Setter, who shares [five ways to slim your Docker images][link-7]. Both articles contain good, albeit somewhat basic, information. Folks who are newer to building Docker images should find these articles useful.
* Apparently there are [more than just a couple different ways to run containers on AWS][link-12].
* Atomic Host will now include `firewalld`; more information is available from Dusty Mabe [here][link-24].
* The Windows Server team managed to shrink the size of the Windows Server Core container image by about 60%; get more details [here][link-26].

## Storage

* Nick Korte [shares some "lessons learned"][link-11] as his team explored VMware vSAN as a storage solution.
* Steve Tilkens has [a review of some different storage options][link-13] he's explored in his VMware-based home lab.
* Tom Twyman has a few recommendations on [increasing vSAN resynch/rebuild performance][link-15].

## Virtualization

* Ray Heffer has [an updated set of Visio stencils and PowerPoint icons][link-9], as well as some links to other sets.
* Ed Haletky has [an updated Linux version of the VMware Software Manager][link-10].
* Alan Renouf [blogged about a new look for vCheck][link-28] (now leveraging the Clarity UI framework).

## Career/Soft Skills

* OK, so this isn't really a "soft skill" _per se_, but it may prove useful nevertheless (and it didn't fit anywhere else). Nick Janetakis writes about [accessing documentation directly from your code editor][link-27] using either Dash (macOS) or Zeal (open source, multiple platforms). I've been a Dash user for a while and I can attest to how handy it can be.

And that's another Tech Short Take in the bag. I hope you found something useful here! As always, feel free to [hit me up on Twitter][link-29] if you have any feedback (or even if you just want to chat).



[link-1]: https://blogs.vmware.com/networkvirtualization/2017/09/nsx-t-routing-where-you-need-it.html/
[link-2]: https://blogs.vmware.com/networkvirtualization/2018/01/nsx-t-routing-part-2.html/
[link-3]: https://blogs.vmware.com/networkvirtualization/2018/01/nsx-t-openapi-sdks.html/
[link-4]: https://cloudonaut.io/migrating-to-amazon-linux-2/
[link-5]: https://www.definit.co.uk/2018/01/nsx-6-x-network-communications-diagram/
[link-6]: https://medium.com/@gabber12/tips-for-writing-dockerfile-b3e569c3134d
[link-7]: https://blog.codacy.com/five-ways-to-slim-your-docker-images-5f4bd68d29f1
[link-8]: https://medium.com/kokster/scheduling-in-kubernetes-part-2-pod-affinity-c2b217312ae1
[link-9]: https://www.rayheffer.com/vmware-visio-stencils-powerpoint-icons-2018/
[link-10]: https://www.astroarch.com/2018/01/vsphere-upgrade-saga-linux-vmware-software-manager-vsm/
[link-11]: http://blog.thenetworknerd.com/2018/01/28/journey-to-vsan-a-technical-adventure/
[link-12]: https://hackernoon.com/7-ways-to-do-containers-on-aws-532f812196f1
[link-13]: http://www.tilkens.com/2018/01/storage-review/
[link-14]: http://www.routetocloud.com/2017/10/introduction-to-nsx-and-kubernetes/
[link-15]: https://fr0gger03.wordpress.com/2018/02/04/increasing-vsan-resynch-rebuild-performance/
[link-16]: https://blog.agilebits.com/2018/01/25/terraforming-1password/
[link-17]: http://vmusketeers.com/2018/02/01/rebuilding-your-lab-in-ravello-is-easy-and-takes-no-time/
[link-18]: https://keepingitclassless.net/2018/02/intentional-infrastructure/
[link-19]: https://jlordiales.me/2018/01/30/cf-gotchas-sg/
[link-20]: https://www.cilium.io/blog/2018/2/6/cilium-troubleshooting-cluster-health-monitor
[link-21]: http://blog.simonmetzger.de/2018/02/salt-napalm-k8s-network-automation/
[link-22]: https://vxsan.com/ansible-vault-passwords-done-easy-with-lastpass/
[link-23]: https://blog.openai.com/scaling-kubernetes-to-2500-nodes/
[link-24]: https://www.projectatomic.io/blog/2018/02/firewalld-in-atomic-host/
[link-25]: http://networkstatic.net/measuring-network-bandwidth-using-iperf-and-docker/
[link-26]: https://blogs.technet.microsoft.com/virtualization/2018/01/22/a-smaller-windows-server-core-container-with-better-application-compatibility/
[link-27]: https://nickjanetakis.com/blog/access-documentation-from-your-code-editor-with-dash-or-zeal
[link-28]: http://www.virtu-al.net/2018/01/08/new-year-new-look-vcheck/
[link-29]: https://twitter.com/scott_lowe
