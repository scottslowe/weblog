---
author: slowe
categories: Information
comments: true
date: 2017-04-07T00:00:00Z
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- Automation
- Linux
- NSX
- VMware
- Terraform
- vSphere
- Windows
- Microsoft
- Docker
- Photon
- RedHat
- Fusion
- PowerCLI
- CLI
title: 'Technology Short Take 81'
url: /2017/04/07/technology-short-take-81/
---

Welcome to Technology Short Take #81! I have another collection of links, articles, and thoughts about key data center technologies, and hopefully I've managed to include something here that will prove useful or thought-provoking. Enjoy!

## Networking

* Matt Oswalt has [a great post][link-4] on the need for networking professionals to learn basic scripting skills---something he (rightfully) distinguishes from programming as a full-time occupation. This is a post well worth spending a few minutes to read, and I wholeheartedly agree with Matt's position here. Further, he follows that up with [a post on how automation is more than just configuration management][link-19]; it's about network services. (This is a view [shared by Ivan Pepelnjak][link-20].) Excellent stuff!
* Speaking of Ivan, he pointed out this post with [45 Wireshark challenges to help you improve your network analysis skills][link-21]. Thanks to Ivan for the tip, and thanks to Johannes Weber for the challenges.
* Jason Edelman also jumps into this discussion on the need for automation/programming/scripting skills with a post that advocates the position of ["automate when you can, program when you must"][link-31]. Jason's position aligns well with the viewpoints of both Matt and Ivan. I must say---if three well-known networking professionals who are also skilled in automation are saying the same thing, you'd probably better pay attention.
* Humair Ahmed of VMware [shares some details][link-11] on a new control plane resiliency feature recently added to VMware NSX: Controller Disconnected Operation (CDO) mode.
* Here's a handy list of [deprecated Linux network commands and their replacements][link-22].
* Yves Fauser discusses [NSX integration with Kubernetes][link-26] in this blog post. If you were at VMworld 2016 last year, this is the integration we demoed **live** in [this Spotlight Session][link-27].

## Servers/Hardware

* Intel NUC or SuperMicro E200-8D? Tai Ratcliff [lays out his rationale][link-18] for going with the latter for his home lab.

## Security

* Konstantin Ryabitsev has a series going on securing a SysAdmin Linux workstation. [Part 1][link-23] covers how to choose a Linux distribution, and [part 2][link-24] discusses some security tips for installing Linux on your SysAdmin workstation.

## Cloud Computing/Cloud Management

* For those that may be new to Terraform, Cody Bunch has a great article that describes [a simple use case of Terraform with vSphere][link-12]. (Yes, folks, you _can_ use "infrastructure as code" techniques and principles with vSphere!)

## Operating Systems/Applications

* A [command-line client for Twitter][link-1]? Yes, please! (I haven't had the chance to actually test this myself, but I do plan to do so in the very near future.)
* Like it or not, systemd looks like it's here to stay. Here's a quick post on [writing systemd units][link-3].
* Brandon Gordon [shares][link-5] how to use VMware Harbor and VMware Admiral in a vCloud Air Network (vCAN) environment for container management.
* Ben Armstrong [shows you][link-6] how to use PowerShell to give your Windows server a proper fully-qualified domain name.
* Check out [this post][link-8] to learn more about _Learning PowerCLI, Second Edition_---this looks like it could be a great resource to "level up" your PowerCLI skills.
* You want to better understand containers? Start [here][link-10] with understanding that containers are, in Jessie Frazelle's words, "not a thing."
* JJ Asghar takes a look at [using VMware Photon OS as the backend Docker host OS for use with test-kitchen][link-13].
* If you're working with PowerShell Core (the multi-platform release of PowerShell) and want some help keeping it up to date, check out [this post][link-14] by Alan Renouf.
* Here's a Windows-centric walkthrough to [using Nginx to load balance across a Docker Swarm cluster][link-15]. (Normally you'd see this sort of thing talking about Linux, but this one uses Windows throughout.)
* Project Harbor, an enterprise-class container registry, has really been seeing some uptake in the community. Check out [this post][link-17] for the latest update.
* Red Hat discusses efforts around [containerizing open-vm-tools][link-28] to better support the use of Atomic OS instances running on VMware infrastructure. Also on the Red Hat front, they recently [announced][link-29] a container base image based on Red Hat Enterprise Linux Atomic. This means you can (potentially) use Atomic OS as the host OS as well as the base OS within containers on that host.

## Storage

* Brian Ragazzi [shares][link-2] a lesson learned the hard way regarding VVols: place the VSM/VASA on a non-VVol storage location.
* The new storage startup Excelero---as described in [this post][link-30] by Chanaka Ekanayake ("Chan")---certainly looks interesting. (As an aside, I do find it fascinating that new storage startups often emerge with support for Linux first, then add other operating systems afterward.)

## Virtualization

* If you're not embracing automation in some form or fashion in your daily tasks, you're not being as effective as you could be. This doesn't necessarily mean writing code of some sort---Cody Bunch demonstrates [how to use packer.io with vSphere][link-7] to help automate building VM images.
* William Lam throws out a teaser about a new project he's been working on with Alan Renouf. [This blog post][link-16] provides a few scant details about the new project, called "USB to SDDC".
* Need to customize the virtual networks that VMware Fusion offers? Matt Behrens [shows how][link-25].

## Career/Soft Skills

* With [this post][link-9], Ethan Banks reminds us tech-centric folks that it's always personal to someone. Keep that in mind---try to always offer constructive feedback instead of just being critical.

OK, it's time to wrap this thing up. Surprisingly, I _actually_ managed to keep this Technology Short Take from getting too long! I wouldn't count on this trend continuing, though...

Take care, folks!



[link-1]: https://sferik.github.io/t/
[link-2]: https://brianragazzi.wordpress.com/2017/03/17/a-downside-to-vvols/
[link-3]: http://hokstadconsulting.com/devops/writing-systemd-units
[link-4]: https://keepingitclassless.net/2017/03/learn-programming-or-perish/
[link-5]: https://blogs.vmware.com/vcat/2017/03/hybrid-container-management-vcloud-director-vmware-harbor.html
[link-6]: https://blogs.msdn.microsoft.com/virtual_pc_guy/2017/03/10/giving-a-workgroup-server-an-fqdn/
[link-7]: http://blog.codybunch.com/2017/03/08/packerio-with-vSphere/
[link-8]: https://blogs.vmware.com/PowerCLI/2017/03/learning-powercli-second-edition.html
[link-9]: http://ethancbanks.com/2017/03/24/its-personal/
[link-10]: https://blog.jessfraz.com/post/containers-zones-jails-vms/
[link-11]: https://blogs.vmware.com/networkvirtualization/2017/03/nsx-v-6-3-control-plane-resiliency-cdo-mode.html#.WOKoNEfiv6U
[link-12]: http://blog.codybunch.com/2017/03/08/A-Simple-Terraform-on-vSphere-Build/
[link-13]: https://jjasghar.github.io/blog/2017/03/29/photonos-as-your-backend-for-kitchen-docker/
[link-14]: http://www.virtu-al.net/2017/03/27/powershell-core-date/
[link-15]: https://blogs.technet.microsoft.com/virtualization/2017/03/17/use-nginx-to-load-balance-across-your-docker-swarm-cluster/
[link-16]: http://www.virtuallyghetto.com/2017/04/project-usb-to-sddc-part-1.html
[link-17]: http://www.think-foundry.com/project-harbor-reaches-milestone-2000-stars-github/
[link-18]: https://lab-rat.com.au/2017/04/01/supermicro-vs-intel-nuc/
[link-19]: https://keepingitclassless.net/2017/04/cheese-moved-long-time-ago/
[link-20]: http://blog.ipspace.net/2017/04/network-automation-is-much-more-than.html
[link-21]: https://blog.webernetz.net/2017/03/29/wireshark-layer-2-3-pcap-analysis-w-challenges-ccnp-switch/
[link-22]: https://dougvitale.wordpress.com/2011/12/21/deprecated-linux-networking-commands-and-their-replacements/
[link-23]: https://www.linux.com/news/linux-workstation-security/2017/3/how-choose-best-linux-distro-sysadmin-workstation-security
[link-24]: https://www.linux.com/news/linux-workstation-security/2017/3/security-tips-installing-linux-your-sysadmin-workstation
[link-25]: https://spin.atomicobject.com/2017/04/03/vmware-fusion-custom-virtual-networks/
[link-26]: https://blogs.vmware.com/networkvirtualization/2017/03/kubecon-2017.html#.WOaT7kfisUH
[link-27]: https://www.youtube.com/watch?v=3xHFK_8Ba58
[link-28]: https://developers.redhat.com/blog/2017/03/23/containerizing-open-vm-tools-part-1-the-dockerfile-and-constructing-a-systemd-unit-file/
[link-29]: http://rhelblog.redhat.com/2017/03/13/introducing-the-red-hat-enterprise-linux-atomic-base-image/
[link-30]: https://chansblog.com/excelero-the-latest-software-defined-storage-startup/
[link-31]: http://jedelman.com/home/automate-when-you-can-program-when-you-must/
