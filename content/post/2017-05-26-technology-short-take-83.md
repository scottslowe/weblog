---
author: slowe
categories: Information
comments: true
date: 2017-05-26T00:00:00Z
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- Ansible
- Automation
- StackStorm
- Salt
- NSX
- VMware
- Linux
- OpenStack
- Docker
- Git
title: 'Technology Short Take 83'
url: /2017/05/26/technology-short-take-83/
---

Welcome to Technology Short Take #83! This is a slightly shorter TST than usual, which might be a nice break from the typical information overload. In any case, enjoy!<!--more-->

## Networking

* I enjoyed Dave McCrory's series on the future of the network (see [part 1][link-5], [part 2][link-6], [part 3][link-7], and [part 4][link-18]---part 5 hadn't gone live yet when I published this). In my humble opinion, he's spot on in his viewpoint that network equipment is increasingly becoming more like servers, so why not embed services and functions in the network equipment? However, this isn't enough; you also need a strong control plane to help manage and coordinate these services. Perhaps [Istio][link-25] will help provide that control plane, though I suspect something more will be needed.
* Michael Kashin has a handy little tool that functions like `ssh-copy-id` on servers, but for network devices (leveraging Netmiko). Check out [the GitHub repository][link-12].
* Anthony Shaw has [a good comparison of Ansible, StackStorm, and Salt][link-13] (with a particular view at applicability in a networking context). This one is definitely worth a read, in my opinion.
* Miguel Gómez of Telefónica Engineering discusses [maximizing performance in VXLAN overlay networks][link-14].
* Nicolas Michel has a good post on [the transition from network engineer v1.0 to v2.0][link-17], and some of the skillsets he believes network engineers must embrace moving forward.

## Servers/Hardware

* Stephen Foskett [shares some details][link-19] on a nifty side project turning an old Macintosh SE into a Core i7 "Hackintosh". Fun read if you're into custom builds.

## Security

* John Welsh has a post on [why someone might use VMware NSX][link-4]. Rich Stroffolino has [a similar piece over at Gestalt IT][link-15], though Rich points out that this use case is only one aspect of how NSX might be used in an environment. These viewpoints---that security via micro-segmentation is a compelling use case for NSX---is one I've seen repeated in numerous environments.
* Here's [a quick post][link-9] on `nftables`, the (eventual) Linux replacement for `iptables`.
* Turns out there's a security vulnerability in some of the wireless keyboard and mice out there. Unfortunately, if you were the owner of a Logitech device, you had to boot into Windows just to update the firmware---until now. Check out [this blog][link-16] on work done to bring firmware updates into the land of Linux.

## Cloud Computing/Cloud Management

* "Even with public cloud and managed services, you can't avoid good old-fashioned data management processes." That's a great quote from Chris Evans discussing some [lessons that can be learned from the Instapaper outage][link-20]. Cloud computing does make some things easier---but it doesn't eliminate other necessary tasks and design considerations.
* Rob Hirschfeld has a wrap-up of day 1 of the recent OpenStack Summit in Boston; read it [here][link-21]. I like Rob's take for a few different reasons. First, Rob's been involved in OpenStack for quite a while; that gives him some perspective. Second, he's very pragmatic about OpenStack; I think he manages to strike a balanced view (not the "rah-rah" from the cheerleaders nor the doom-and-gloom from the pundits).

## Operating Systems/Applications

* I was recently reviewing some articles on Git, the popular distributed version control system, and came across a few related posts. First, there was [this post against GitFlow][link-1], followed by [this post on OneFlow][link-2] (a potential replacement for GitFlow). Not having used Git or GitFlow in large development environments (only for personal projects), I can't speak to the author's criticisms, but they certainly seem valid. I also saw [this proposed Git workflow for "infrastructure-as-code" projects][link-3], which I'm going to review in more detail to try to better understand.
* I found [this post][link-10] describing someone's migration from macOS to OpenBSD. While I've played around a fair amount with OpenBSD on the server side, I (personally) wouldn't have considered it as a desktop OS. However, the author of this post seems to be getting along pretty well.
* Also on the topic of migrating to different OSes, here's a post on [using GalliumOS on a Chromebook][link-11].
* I must confess I was, at first, a bit confused about [the Moby project][link-22]. After a little time and seeing how things seem to be progressing, I realize that it creates a clear separation between the open source project formerly known as Docker (now Moby) and Docker CE/EE (the products from Docker Inc.). This was the source of some confusion in the past; the move to Moby sidesteps those concerns.

## Storage

Nothing this time around, but I'm sure I'll find content to include in future episodes.

## Virtualization

* Luc Dekens [explores][link-23] the open-sourced vSphere Automation SDKs [announced back in March][link-24] (I think I may have referenced this already in a previous TST). Naturally, Luc's exploration primarily focuses on PowerCLI.

## Career/Soft Skills

* Ivan Pepelnjak has [a few secrets of successful learning][link-8] that any technology professional can leverage to his/her advantage.

That's all this time around---I hope you found something useful here.

[link-1]: http://endoflineblog.com/gitflow-considered-harmful
[link-2]: http://endoflineblog.com/oneflow-a-git-branching-model-and-workflow
[link-3]: https://github.com/joemiller/git-flux
[link-4]: http://samplefive.com/2017/05/why-the-heck-would-you-use-nsx/
[link-5]: https://blog.mccrory.me/2017/05/22/the-future-of-the-network/
[link-6]: https://blog.mccrory.me/2017/05/22/the-future-of-the-network-part-2/
[link-7]: https://blog.mccrory.me/2017/05/23/beyond-the-network-the-future-of-the-network-part-3/
[link-8]: http://blog.ipspace.net/2017/05/few-secrets-of-successful-learning.html
[link-9]: http://ral-arturo.org/2017/05/05/debian-stretch-stable-nftables.html
[link-10]: https://mndrix.blogspot.com/2017/05/switching-to-openbsd.html
[link-11]: https://blog.roetert.eu/linux-on-chromebook/
[link-12]: https://github.com/networkop/ssh-copy-net
[link-13]: https://medium.com/@anthonypjshaw/ansible-v-s-salt-saltstack-v-s-stackstorm-3d8f57149368
[link-14]: https://engineering.telefonica.com/maximizing-performance-in-vxlan-overlay-networks-ec35ebe29440
[link-15]: http://gestaltit.com/exclusive/rich/vmware-nsx-going-big-micro-segmentation/
[link-16]: https://blogs.gnome.org/hughsie/2017/05/22/updating-logitech-hardware-on-linux/
[link-17]: http://vpackets.net/2017/05/network-engineer-v1-0-v2-0/
[link-18]: https://blog.mccrory.me/2017/05/25/networks-openness-istio-the-future-of-networks-part-4/
[link-19]: http://blog.fosketts.net/2017/05/25/core-i7-macintosh-se
[link-20]: https://blog.architecting.it/2017/03/26/learning-from-the-instapaper-outage/
[link-21]: https://robhirschfeld.com/2017/05/09/openstack-boston-day-1-notes/
[link-22]: https://blog.docker.com/2017/04/introducing-the-moby-project/
[link-23]: http://www.lucd.info/2017/03/10/vsphere-automation-sdks-powershell-part-1/
[link-24]: https://blogs.vmware.com/opensource/2017/03/09/integration-vmware-vsphere-using-new-open-sourced-software-development-kits/
[link-25]: https://istio.io/
