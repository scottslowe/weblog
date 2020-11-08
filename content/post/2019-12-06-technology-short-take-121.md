---
author: slowe
categories: Information
comments: true
date: 2019-12-06T09:30:00-07:00
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- NSX
- KVM
- macOS
- Docker
- Linux
- Kubernetes
- AWS
- VPN
- Terraform
- vSphere
- VMware
title: 'Technology Short Take 121'
url: /2019/12/06/technology-short-take-121/
---

Welcome to Technology Short Take #121! This may possibly be the last Tech Short Take of 2019 (not sure if I'll be able to squeeze in another one), so here's hoping that you find something useful, helpful, or informative in the links that I've collected. Enjoy some light reading over your festive holiday season!<!--more-->

## Networking

* Brian Linkletter has a post on [how to run the Antidote network emulator on KVM for better performance][link-4]. Antidote, as I understand it, is the network emulator that runs the labs on the NRELabs web site (which is another cool thing in and of itself). In perusing some of Brian's content, I see lots of stuff at the intersection of networking and virtualization. That's a good place to be right now, in my opinion.
* Anders Olsson explains [how to map NSX-T UUIDs to the actual logical objects][link-6].
* Julien Demierre has three-part series on interfaces and uplinks for ESXi/Edge in NSX-T deployments ([part 1][link-18], [part 2][link-19], and [part 3][link-20]).
* This is an old(er) post that I've probably mentioned here before, but I think it still has relevance. Brent Salisbury's [post on Golang for Network Ops][link-24] not only points out _why_ networking pros should learn Golang, but also provides some resources on _how_ to learn it.
* Philippe Bogaerts has an article on [how to use `tcpdump` effectively in Kubernetes][link-29]. The one caveat to his approach that may be worth mentioning is that some policies (a PodSecurityPolicy or the use of Open Policy Agent) may prevent the user from launching a Pod connected to the host's network namespace.

## Servers/Hardware

Nothing this time around, although there may have been some hardware-related news coming out of re:Invent that I missed (such as new generations of AWS-designed chips being announced).

## Security

* Joel de la Garza has a post over on the a16z site on [16 steps to securing your data (and life)][link-2].
* SentinelOne discusses [how to spoof privileged helpers to exploit macOS][link-11]. Once again we see that fooling the user is the most reliable way to gain access to a system.
* A massive data leak exposing details on as many as 1.2 _billion_ (yes, you read that right) people? Ouch. See [this post][link-16] for details.
* Whiskey Tango brings [a privacy issue with Keybase][link-21] to light (and an issue that I've experienced as a Keybase user).
* A [new VPN security flaw][link-26] has been uncovered that affects a wide range of UNIX- and Linux-based systems.

## Cloud Computing/Cloud Management

* I found [this article on chaos engineering][link-8] to be helpful in understanding the principles and concepts. The article also pointed me to [this GitHub repository with chaos engineering resources][link-10].
* [This GitHub blog post][link-13] tells the story of the extremely in-depth debugging required to track down the source of some network stalls in Kubernetes.
* Taylor Clauson [takes an investment-centric view][link-17] of AWS, its ever-growing list of services and offerings, and the evolving cloud computing market.
* Kief Morris' article on [breaking up your Terraform project][link-27] (day 5 of SysAdvent) is really good; well worth the read, in my opinion.

## Operating Systems/Applications

* I found this tidbit on Twitter ([here's the original tweet][link-1]) but wanted to share it here. Apparently you can get a terminal into your Docker Desktop for Mac VM by running `screen ~/Library/Containers/com.docker.docker/Data/vms/0/tty`. This will give you a root prompt for the VM behind Docker Desktop. Handy!
* The latest version of macOS ("Catalina") drops `bash` as the default shell in favor of `zsh`. I guess this is fine, since the version of `bash` that ships with macOS is two major releases behind (due to Apple's aversion to all things GPL). To help you make the most of your `zsh` environment, check out [this repository by Justin Garrison][link-3].
* Robert Kloosterhuis talks about [running CoreDNS in a Docker lab to provide DNS for your home lab][link-7].
* Here's an article from Marko Saric on [his switch from macOS to Linux][link-9].
* A complete OS based on the bare bones open source that backs macOS is apparently [a real thing called PureDarwin][link-12].
* [This Ars Technica article][link-14] is originally from November 2013, but resurfaced over the 2019 Thanksgiving holiday---it tells the story of OS/2.
* Samuel Karp has [a list of guidelines for designing and testing software daemons][link-15].
* Jamie Duncan (a new teammate of mine) writes about [domain-specific DNS settings on macOS][link-22].
* Luc Dekens has [a great article on the basics of `cloud-init`][link-28], with a particular focus on its usage with PowerCLI in VMware-based environments.

## Storage

Nothing this time; sorry! I'll be on the lookout for stuff I can include next time around.

## Virtualization

* Are you running an all-VMware shop and want to add some Kubernetes to the mix? [This blog post][link-5] has a link to a white paper that could be useful.
* Phil Chapman talks about [using PowerCLI to modify settings for an existing Horizon View pool][link-22].
* Maarten Van Driessen [shares a workaround][link-25] for an error updating standalone ESXi hosts to version 6.5.

## Career/Soft Skills

I haven't included anything this time, as this Short Take has been pretty heavy on technical content. I'll stay alert for content to include here next time.

OK, that's it for now! I hope this was helpful and informative. Feedback is always welcome; it's probably easiest [to contact me via Twitter][link-99], but I'm around a lot of different places if you look. Have a great weekend!

[link-1]: https://twitter.com/micahhausler/status/1197661986458198016
[link-2]: https://a16z.com/2019/11/12/16-steps-secure-you/
[link-3]: https://github.com/rothgar/mastering-zsh
[link-4]: https://www.brianlinkletter.com/run-the-antidote-network-emulator-on-kvm-for-better-performance/
[link-5]: https://blogs.vmware.com/apps/2019/11/best-practices-for-enterprise-kubernetes-on-the-vmware-sddc.html
[link-6]: https://rtsab.com/rts_blogg/nsx-t-how-to-figure-out-what-those-weird-uuids-really-represent/
[link-7]: http://thefluffyadmin.net/?p=1427
[link-8]: https://www.gremlin.com/community/tutorials/chaos-engineering-the-history-principles-and-practice/
[link-9]: https://markosaric.com/linux/
[link-10]: https://github.com/dastergon/awesome-chaos-engineering
[link-11]: https://www.sentinelone.com/blog/macos-red-team-spoofing-privileged-helpers-and-others-to-gain-root/
[link-12]: https://www.jamieweb.net/blog/a-look-at-puredarwin/
[link-13]: https://github.blog/2019-11-21-debugging-network-stalls-on-kubernetes/
[link-14]: https://arstechnica.com/information-technology/2019/11/half-an-operating-system-the-triumph-and-tragedy-of-os2/
[link-15]: https://samuel.karp.dev/blog/2019/11/software-daemons/
[link-16]: https://www.dataviper.io/blog/2019/pdl-data-exposure-billion-people/
[link-17]: https://www.tclauson.com/2019/09/11/Unbundling-AWS.html
[link-18]: https://www.vm-spec.ch/2019/12/01/interfaces-uplinks-for-esxi-edge-part-1/
[link-19]: https://www.vm-spec.ch/2019/12/01/interfaces-uplinks-for-esxi-edge-part-2/
[link-20]: https://www.vm-spec.ch/2019/12/01/interfaces-uplinks-for-esxi-edge-part-3/
[link-21]: https://www.whiskey-tango.org/2019/11/keybase-weve-got-privacy-problem.html
[link-22]: https://medium.com/@jamieeduncan/i-recently-moved-to-a-macbook-for-my-primary-work-laptop-7c704dbaff59
[link-23]: https://philchapman.us/2019/12/02/horizon-powercli-modify-existing-pool-settings/
[link-24]: http://networkstatic.net/golang-network-ops/
[link-25]: https://www.brisk-it.net/update-esxi-fails-with-dependency-error-vmkapi_2_0_0_0/
[link-26]: https://thehackernews.com/2019/12/linux-vpn-hacking.html
[link-27]: https://sysadvent.blogspot.com/2019/12/day-5-break-up-your-terraform-project.html
[link-28]: http://www.lucd.info/2019/12/06/cloud-init-part-1-the-basics/
[link-29]: https://medium.com/@xxradar/how-to-tcpdump-effectively-in-kubernetes-part-2-7e4127b42dc7
[link-99]: https://twitter.com/scott_lowe
