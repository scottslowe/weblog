---
author: slowe
categories: Information
comments: true
date: 2008-08-11T23:03:35Z
slug: virtualization-short-take-16
tags:
- ESX
- ESXi
- VCB
- Virtualization
- VMotion
- VMware
- VMwareHA
title: 'Virtualization Short Take #16'
url: /2008/08/11/virtualization-short-take-16/
wordpress_id: 812
---

There's a lot of good information being shared out there! With all due credit to the original authors, here's a few links that I found particularly interesting or useful. I hope you agree!

* The VI Team Blog posted a list of [interesting things in Update 2](http://blogs.vmware.com/vi/2008/08/interesting-ite.html).  They identify things like Enhanced VMotion Compatibility (EVC) and monitoring and availability enhancements. For example, did you know that VM failure monitoring is now fully supported and not experimental? One thing that caught my eye when I first [announced Update 2][1] was VSS support. What I hadn't really noted about this was the fact that this extends into the application layer, meaning that VSS-aware applications will be quiesced for VCB-based backups. This brings application data consistency to VCB-based backups, and this is a _big deal._

* And while we're discussing the VI Team Blog, have a quick look at [part 1](http://blogs.vmware.com/vi/2008/07/top-tips-for-de.html) and [part 2](http://blogs.vmware.com/vi/2008/08/top-tips-for-de.html) of their tips for deploying VI. The discussion of Fibre Channel path selection policy with active/passive storage arrays is particularly helpful.

* Rich over at VM /ETC brought some interesting facts to my attention regarding the free version of ESXi in [this post](http://vmetc.com/2008/08/10/whats-the-difference-between-free-esxi-and-licensed-esxi/). I was not aware, for example, that the Remote CLI was read-only with free ESXi. Very interesting, and quite useful.

* Duncan at Yellow Bricks has also been pumping out some very helpful information, with [an update](http://www.yellow-bricks.com/2008/08/01/update-ha-advanced-options/) on HA advanced options and a pointer to a document describing [what happens if VirtualCenter crashes](http://www.yellow-bricks.com/2008/08/05/what-if-my-virtualcenter-server-crashes/). One of the HA options that caught my eye was an option that allows VMotion interfaces to be used for HA. This means that we can easily provide additional HA redundancy simply by configuring VMotion interfaces. Handy!

* Chris Wolf brings to light [information on PXE booting ESXi](http://www.chriswolf.com/?p=182), and in so doing discusses the idea of the "stateless hypervisor." This is an idea that is gaining a lot of ground, also being the subject of the [release of a utility](http://www.vinternals.com/2008/08/announcing-statelesx-100.html) designed specifically around that very idea (but for ESX, not ESXi). When I design a VI environment, I'm already treating the hypervisor as mostly stateless; all the data and "important" information is stored on the SAN. To fully embrace the idea of a stateless hypervisor, we also need to incorporate auto-configuration.

That's it for now. If any readers have any interesting links they'd like to share, please do so in the comments below. Thanks!

[1]: {{< relref "2008-07-26-vmware-releases-update-2.md" >}}
