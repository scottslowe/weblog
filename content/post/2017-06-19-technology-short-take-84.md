---
author: slowe
categories: Information
comments: true
date: 2017-06-19T00:00:00Z
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- VXLAN
- Geneve
- OVN
- NSX
- VMware
- Kubernetes
- Consul
- Linux
- Docker
- Terraform
- OpenStack
- AWS
- Windows
- Microsoft
- Fedora
- VirtualBox
title: 'Technology Short Take 84'
url: /2017/06/19/technology-short-take-84/
---

Welcome to Technology Short Take #84! This episode is a bit late (sorry about that!), but I figured better late than never, right? OK, bring on the links!<!--more-->

## Networking

* When I joined the NSX team in early 2013, a big topic at that time was overlay protocols (VXLAN, STT, etc.). Since then, that topic has mostly faded, though it still does come up from time to time. In particular, the move toward Geneve has prompted that discussion again, and Russell Bryant tackles the discussion in [this post][link-3].
* Sjors Robroek [describes his nested NSX-T lab][link-8] that also includes some virtualized network equipment (virtualized Arista switches). Nice!
* Colin Lynch shares some details on [his journey with VMware NSX][link-16] (so far).
* I wouldn't take this information as gospel, but [here's][link-29] a breakdown of some of the IPv6 support available in VMware NSX.

## Servers/Hardware

* Here's [an interesting article][link-14] on the role that virtualization is playing in the network functions virtualization (NFV) space now that ARM hardware is growing increasingly powerful. This is a space that's going to see some pretty major changes over the next few years, in my humble opinion.

## Security

* Anthony Burke gives a little bit of a sneak peek at some functionality from the upcoming v3 release of PowerNSX: [searching NSX Distributed Firewall (DFW) rules][link-21].
* Yes, I know that security and privacy are different, but this seems like a reasonable place to put this article by J Metz [on pseudo-anonymous e-mail][link-27].

## Cloud Computing/Cloud Management

* Here's [a good post on Kubernetes Ingress][link-4] by Jay Gorrell.
* Timothy Perrett has [an article on Envoy with Nomad and Consul][link-12]. The article supplies lots of useful information, but I really would've liked some concrete examples of how to glue these pieces together.
* Voice control for your vSphere lab using Amazon Echo? Cody De Arkland [shows you how][link-13]. Also check out [this follow-up post][link-17] and [a companion post][link-18] by William Lam. Interesting stuff here!
* John Troyer of TechReckoning shares [some of his thoughts on OpenStack][link-15] following his attendance at the Boston OpenStack summit.
* Kamal Marhubi shares some [Terraform gotchas and the workarounds][link-24] used to deal with them. There's some good information here for Terraform users.
* Bernard Golden makes some good points [here][link-26]; a bit long but worth a read.
* In [this article][link-30], Steve Chambers share some good reasons to use the AWS CLI and some "funky" uses. I don't think the uses were all that "funky," but that's OK---there's still some good information here for CLI lovers such as myself.

## Operating Systems/Applications

* If you're interested in getting started with Prometheus, you might give [this article][link-2] by Scott Collier a quick read; it has some instructions for "kicking the tires".
* Here's a "back to the basics" post with an explanation of [what an init system is][link-5] (uses Fedora as an example).
* This post has [a list of 5 Docker utilities][link-7] you may find helpful.
* I must confess I hadn't considered the idea of [using Terraform to manage GitHub][link-9].
* Aside from a Windows VM I maintain for the occasional thing I can't do effectively on Linux or OS X, I haven't worked with Windows in any significant capacity in quite a while. However, I know that a lot of my readers use Windows and need to support Windows, so I'm hoping that including Cody Bunch's post on [building a Windows domain with BoxStarter][link-10] will prove helpful to someone. This other post by Cody on [automated Windows 10 post-install][link-11] may also be useful.
* [This post][link-20] by Jason Valentino describing the move of Capital One's largest customer-facing applications to AWS is a really good read. I'd like to stress---as Valentino does---that a lot of what made this effort so successful was that the development team was already "cloud ready". Culture, process, and mindset are often more important than technology.
* Looking for some Docker and Kubernetes resources? Check out James Thorne's [recent post on beginner resources for Docker and Kubernetes][link-22].
* Dieter Reuter shares the results of some testing---a "technical POC", he calls it---with [LinuxKit and AWS][link-25].
* [RegEx tester][link-31]. Enough said.

## Storage

Nothing this time around, but I've had some volunteers offer to share some links for next time around. (If you have links you'd like to share, for this section or _any_ section, just drop me a message!)

## Virtualization

* Here's [a quick post][link-1] on using Fedora 25 as a hypervisor, leveraging KVM (part of the Linux kernel, as you probably already know) and a tool called Virtual Machine Manager.
* Going to be at VMworld and want to give back to the community? [Here's a great way.][link-23]
* [This article][link-28] has a few good tips for VirtualBox, but overall---to be honest---I was hoping for more.

## Career/Soft Skills

* I really enjoyed this article by Jon Hall on [the Andon cord and ITSM's DevOps challenge][link-6]. Insightful and informative posts like this go a long way, in my book, to convincing people why DevOps is culture more than tooling (vendors often have it the other way around).
* Looks like I'm [not the only one][link-19] recommending that folks take some time to learn Git.

That's all for this time, but don't worry---I'll be back in a couple of weeks (approximately) with more links, articles, and thoughts to share. Until then, enjoy!



[link-1]: https://strikerttd.wordpress.com/2017/05/31/configuring-fedora-25-as-a-hypervisor-using-virtual-machine-manager/
[link-2]: http://www.colliernotes.com/2017/06/kicking-tires-of-prometheus-using.html
[link-3]: https://blog.russellbryant.net/2017/05/30/ovn-geneve-vs-vxlan-does-it-matter/
[link-4]: https://medium.com/@cashisclay/kubernetes-ingress-82aa960f658e
[link-5]: https://fedoramagazine.org/what-is-an-init-system/
[link-6]: https://medium.com/@JonHall_/the-andon-cord-and-itsms-devops-challenge-78395393c56f
[link-7]: https://blog.xebialabs.com/2017/05/18/5-docker-utilities-you-should-know/
[link-8]: https://vxsan.com/building-a-nsx-t-nested-lab-2/
[link-9]: https://www.hashicorp.com/blog/managing-github-with-terraform/
[link-10]: http://blog.codybunch.com/2017/04/10/Building-a-Windows-Domain-with-PowerShell/
[link-11]: http://blog.codybunch.com/2017/03/27/Basic-Automated-Windows-10-post-install/
[link-12]: http://timperrett.com/2017/05/13/nomad-with-envoy-and-consul
[link-13]: https://www.thehumblelab.com/integrating-echo-vmware/
[link-14]: https://semiengineering.com/carriers-push-datacenter-style-virtualization/
[link-15]: https://medium.com/the-techreckoning-dispatch/its-turtles-all-the-way-down-for-openstack-5057ac07db20
[link-16]: https://ucsguru.com/2017/04/18/my-journey-with-vmware-nsx/
[link-17]: https://www.thehumblelab.com/gideon-clarityui-interface-into/
[link-18]: https://www.virtuallyghetto.com/2017/06/introducing-alexa-to-a-few-more-vmware-apis.html
[link-19]: http://blog.ipspace.net/2017/06/want-to-learn-something-new-learn-git.html
[link-20]: https://medium.com/capital-one-developers/moving-one-of-capital-ones-largest-customer-facing-apps-to-aws-668d797af6fc
[link-21]: http://networkinferno.net/searching-firewall-rules-with-powernsx
[link-22]: https://thornelabs.net/2017/06/13/beginning-to-understand-docker-and-kubernetes.html
[link-23]: https://blogs.vmware.com/vmtn/2017/06/call-content-vmworld-vmtn.html
[link-24]: https://heap.engineering/terraform-gotchas/
[link-25]: https://bee42.com/blog/linuxkit-with-initial-aws-support/
[link-26]: http://bernardgolden.com/2017/06/cloud-computing-hits-a-tipping-point/
[link-27]: https://jmetz.com/2017/04/protip-pseudo-anonymous-email/
[link-28]: https://hackernoon.com/virtualbox-are-you-getting-your-moneys-worth-4d7f98f3d7d2
[link-29]: https://www.vmbaggum.nl/2017/01/nsx-ipv6-support/
[link-30]: https://ukcloud.pro/5-fancy-reasons-and-7-funky-uses-for-the-aws-cli-288e0e688030
[link-31]: https://regex101.com/
