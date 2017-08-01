---
author: slowe
categories: Information
comments: true
date: 2011-02-17T18:00:12Z
slug: technology-short-take-11
tags:
- Cisco
- CLARiiON
- CLI
- EMC
- Networking
- Nexus
- Storage
- UCS
- Virtualization
- VMware
- vSphere
title: 'Technology Short Take #11'
url: /2011/02/17/technology-short-take-11/
wordpress_id: 2240
---

That's right folks, it's time for another installation of Technology Short Takes. This is Technology Short Take #11, and I hope that you find the collection of items I have for you this time around both useful and informative. But enough of the introduction---let's get to the good stuff!

## Networking

* David Davis (of Train Signal) has a good write-up on the Petri IT Knowledgebase on [using a network packet analyzer with VMware vSphere](http://www.petri.co.il/using-packet-analyzer-on-virtual-network.htm). The key, of course, is enabling promiscuous mode. Read the article for full details.

* Jason Nash instructs you on [how to enable jumbo frames on the Nexus 1000V](http://jasonnash.wordpress.com/2011/02/06/enabling-jumbo-frames-on-the-cisco-nexus-1000v/), in the event you're interested. Jason also has [good coverage](http://jasonnash.wordpress.com/2011/02/02/virtual-networking-gets-even-better-cisco-nexus-1000v-release-4-21-sv14/) of the latest release of the Nexus 1000V; worth reading in my opinion. Personally, I'd like Cisco to get to version numbers that are a bit simpler than 4.2(1) SV1(4).

* Now here's a link that is truly useful: Greg Ferro has put together a list of [Cisco IOS CLI shortcuts](http://etherealmind.com/cisco-ios-cli-shortcuts/). That's good stuff!

* There are a number of reasons why I have come to generally recommend against link aggregation in VMware vSphere environments, and Ivan Pepelnjak exposes another one that rears its head in multi-switch environments in [this article](http://blog.ioshints.info/2011/01/vswitch-in-multi-chassis-link.html). With the ability for vSphere to utilize all the uplinks without link aggregation, the need to use link aggregation isn't nearly as paramount, and avoiding it also helps you avoid some other issues as well.

* Ivan also tackles [the layer 2 vs. layer 3 discussion](http://blog.ioshints.info/2011/02/layer-3-gurus-asleep-at-wheel.html), but that's beyond my pay grade. If you're a networking guru, then this discussion is probably more your style.

* [This VMware KB article](http://kb.vmware.com/kb/1010691), surprisingly enough, seems like a pretty good introduction to private VLANs and how they work. If you're not familiar with PVLANs, you might want to give this a read.

## Servers

* Want to become more familiar with Cisco UCS, but don't have an actual UCS to use? Don't feel bad, I don't either. But you can use the Cisco UCS Emulator, which is described in a bit more detail by Marcel [here](http://blog.nessus.nl/598/cisco-ucs-emulator/). Very handy!

## Storage

* Ever find yourself locked out of your CLARiiON because you don't know or can't remember the username and password? OK, maybe not (unless you inherited a system from your predecessor), but in those instances [this post](http://goingvirtual.wordpress.com/2011/02/02/locked-out-of-navisphere-oh-crp-now-what) by Brian Norris will come in handy.

* Fabio Rapposelli posted [a good write-up on the internals of SSDs](http://juku.it/en/2010/12/14/ssd-demystified/), in case you weren't already aware of how they worked. As SSDs gain traction in many different areas of storage, knowing how SSDs work helps you understand where they are useful and where they aren't.

* Readers that are new to the storage space might find [this post on SAN terminology](http://www.dasblinkenlichten.com/?p=291) helpful. It is a bit specific to Cisco's Nexus platform, but the terms are useful to know nevertheless.

* If you like's EMC's Celerra VSA, you'll also like [the new Uber VSA Guide](http://nickapedia.com/2011/02/05/how-to-uber-new-celerra-uber-vsa-guide/). See this post over at Nick's site for more details.

* Fellow vSpecialist Tom Twyman posted [a good write-up on installing PowerPath/VE](http://tastytech.t3webinc.com/2011/02/how-to-powerpath-virtual-edition-for-vmware/). It's worth reading if you're considering PP/VE for your environment.

* Joe Kelly of Varrow posted [a quick write-up about VPLEX and RecoverPoint](http://blog.virtualtacit.com/post/3029390798/vplex-and-recoverpoint-upward-and-onward), in which he outlines one potential issue with interoperability between VPLEX and RecoverPoint: how will VPLEX data mobility affect RP? For now, you do need to be aware of this potential issue. For more information on VPLEX and RecoverPoint together, I'd also suggest having a look at [my write-up on the subject][1].

* I won't get involved in the discussion around Open FCoE (the software FCoE stack announced a while back); plenty of others (J Metz speaks out [here](http://blogs.cisco.com/datacenter/is-intels-openfcoe-announcement-a-big-deal/), Chad Sakac weighed in [here](http://virtualgeek.typepad.com/virtual_geek/2011/01/native-open-intel-fcoe-software-stack-game-changer-imo.html), Ivan Pepelnjak offers his opinions [here](http://blog.ioshints.info/2011/01/open-fcoe-software-implementation-of.html), and Wikibon [here](http://wikibon.org/blog/hp-and-intel-help-open-the-fcoe-market/)) have already thrown in. Instead, I'll take the "Show me" approach. Intel has graciously offered me two X520 adapters, which I'll run in my lab next to some Emulex CNAs. From there, we'll see what the differences are under the same workloads. Look for more details from that testing in the next couple of months (sorry, I have a lot of other projects on my plate).

* Jason Boche has been working with Unisphere, and apparently he likes the Unisphere-VMware integration (he's not alone). Check out his write-up [here](http://www.boche.net/blog/index.php/2011/02/14/vsphere-integration-with-emc-unisphere/).

## Virtualization

* For the most part, a lot of people don't have to deal with SCSI reservation conflicts any longer. However, they can happen (especially in older VMware Infrastructure 3.x environments), and in [this post](http://blog.vmpros.nl/2010/12/23/vmware-scsi-reservation-conflicts/) Sander Daems provides some great information on detecting and resolving SCSI reservation conflicts. Good write-up, Sander!

* If you like the information vscsiStats gives you but don't like the format, check out Clint Kitson's [PowerShell scripts for vscsiStats](https://community.emc.com/message/527489).

* And while we're talking vscsiStats, I would be remiss if I didn't mention Gabe's post on [converting vscsiStats data into Excel charts](http://www.gabesvirtualworld.com/?p=1022).

* Rynardt Spies has decided he's [going Hyper-V instead of VMware vSphere](http://virtualvcp.com/news/158-why-im-swapping-vsphere-for-hyper-v). OK, only in his lab, and only to learn the product a bit better. While we all agree that VMware vSphere far outstrips Hyper-V today, Rynardt's decision is both practical and prudent. Keep blogging about your experiences with Hyper-V, Rynardt---I suspect there will be more of us reading them than perhaps anyone will admit.

* Brent Ozar (great guy, by the way) has an enlightening post about some of [the patching considerations around Azure VMs](http://theinfoboom.com/articles/why-azure-vms-will-fail/). All I can say is ouch.

* The NIST has finally issued the final version of full virtualization security guidelines; see [the VMBlog write-up](http://vmblog.com/archive/2011/02/02/nist-issues-final-version-of-full-virtualization-security-guidelines.aspx) for more information.

* vCloud Connector was announced by VMware last week at Partner Exchange 2011 in Orlando. More information is available [here](http://it20.info/2011/02/vmware-vcloud-connector-on-the-way-to-the-hybrid-clouds/) and [here](http://blogs.vmware.com/rethinkit/2011/02/vcloud-connector-makes-hybrid-cloud-management-easy.html).

* Arnim van Lieshout posted an interesting article on [how to configure EsxCli using PowerCLI](http://www.van-lieshout.com/2011/01/esxcli-powercli/).

* Sander Daems gets another mention in this installation of Technology Short Takes, this time for good information on [an issue with ESXi and certain BIOS revisions of the HP SmartArray 410i array controller](http://blog.vmpros.nl/2010/12/23/vmware-esxi-4-1-installation-hangs-on-multiextent-loaded-succesfully-hp-dl380-g7/). The fix is an upgrade to the firmware.

* Sean Clark did some "what if" thinking in [this post about the union of NUMA and vMotion](http://seanclark.us/?p=350) to create VMs that span multiple physical servers. Pretty interesting thought, but I do have to wonder if it's not that far off. I mean, how many people saw vMotion coming before it arrived?

* The discussion of a separate "management cluster" has been getting some attention recently. First was Scott Drummonds, with [this post](http://vpivot.com/2011/02/02/vshield-vcenter-and-management-clusters/) and [this follow up](http://vpivot.com/2011/02/14/vshield-clarification/). Duncan responded [here](http://www.yellow-bricks.com/2011/02/14/management-cluster-vshield-resiliency/). My take? I'll agree with Duncan's final comment that "an architect/consultant will need to work with all the requirements and constraints". In other words, do what is best for your situation. What's right for one customer might not be right for the next.

* And speaking of vShield, be sure to check out Roman Tarnavski's post on [extending vShield](http://blog.romant.net/vmware/extending-vshield/).

* Interested in knowing more about how Hyper-V does CPU scheduling? Ben Armstrong is happy to help out, with [Part 1](http://blogs.msdn.com/b/virtual_pc_guy/archive/2011/02/14/hyper-v-cpu-scheduling-part-1.aspx) and [Part 2](http://blogs.msdn.com/b/virtual_pc_guy/archive/2011/02/15/hyper-v-cpu-scheduling-part-2.aspx) of CPU scheduling with Hyper-V.

* Here's a good write-up on [how to configure Pass-Through Switching (PTS) on UCS](http://vblog.wwtlab.com/2011/02/14/configuring-pass-through-switching-pts-within-ucs-using-the-virtual-interface-card-vic-2/). This is something I still haven't had the opportunity to do myself. It kind of helps to actually have a UCS for stuff like this.

It's time to wrap up now; I think I've managed to throw out a few links and articles that someone should find useful. As always, feel free to speak up in the comments below.

[1]: {{< relref "2010-12-20-using-vplex-and-data-replication-together.md" >}}
