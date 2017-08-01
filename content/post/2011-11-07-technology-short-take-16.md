---
author: slowe
categories: Information
comments: true
date: 2011-11-07T10:00:00Z
slug: technology-short-take-16
tags:
- HyperV
- Microsoft
- Networking
- Storage
- vCloud
- Virtualization
- VMware
- vSphere
- OVS
- VXLAN
- FCoE
title: 'Technology Short Take #16'
url: /2011/11/07/technology-short-take-16/
wordpress_id: 2455
---

Welcome to Technology Short Take #16. It's been quite a while since my last Technology Short Take (a month!), and I don't know if that's a good thing (so readers didn't have to listen to my rambling) or a bad thing (readers missing out on what I hope are useful or interesting links). In any case, here's my latest collection of various data center-related links, articles, and thoughts. Thanks for reading!

## Networking

* A great of my networking-related reading over the last few weeks has been focused on OpenFlow and trying to better understand what it is and how it affects things (both today and in the future). I won't share all of them here (I'll probably post a separate collection of all the links I've gathered), but I did want to mention that briefly. Of particular interest to me is the interaction/integration between OpenFlow, [Open vSwitch](http://openvswitch.org/), and OpenStack. Any notes/thoughts/ideas there that readers would like to share are welcomed.

* While this post on [NVGRE, VXLAN, and what Microsoft is doing right](http://networkheresy.wordpress.com/2011/10/03/nvgre-vlxan-and-what-microsoft-is-doing-right/) is a bit slanted in favor of Open vSwitch, I do agree that standardizing the control plane for managing the virtual networking platform is a worthy goal. We all know, intuitively, that we need better orchestration and more extensive automation; providing a standardized control interface is one step closer to achieving that, in my opinion.

* Ivan has a great treatise on [why virtual switches need BPDU guard](http://blog.ioshints.info/2011/11/virtual-switches-need-bpdu-guard.html). As usual, his post is spot on---with one minor exception. Current recommendations for vSphere HA state that, in most cases, isolation response should be configured to leave VMs powered on. Thus, the scenario he describes in which a misconfigured VM might take down all the links on an ESX/ESXi host and then cause the VMs to be rebooted is far less likely to occur. Even so, that's a minor nit, and the point of the article remains valid and useful.

## Servers

* For a bit of a real-world look at Cisco UCS, read [this post](http://www.chrisatkinson.com/?p=10) by Chris Atkinson, a fairly recent adopter of UCS in his environment.

## Storage

* If you haven't had a chance to catch up with Rob Peglar's "Architecture Matters" series of blog posts, I think it's worth checking out. [Part 1 is here](http://www.isilon.com/blog/architecture-matters) and [part 2 is here](http://www.isilon.com/blog/architecture-matters---part-ii). (Rob, by the way, is the Americas CTO for Isilon.)

* The "readiness" of FCoE for the enterprise is a topic that has come up once again. Stephen Foskett stirred the waters---something that he seems to be doing more frequently now---with [this article](http://blog.fosketts.net/2011/10/21/fcoe-ready-prime-time/). Predictably (and I don't mean that in a bad way), J Metz has come out squarely on the side of "FCoE _is_ ready" (read [his post](http://blogs.cisco.com/datacenter/47589/)); Greg Ferro has come out swinging against FCoE (read [his post](http://etherealmind.com/sell-buy-multi-hop-fcoe-consultants-dream/)). I can see both sides of the argument; personally, I think that these two sides are operating on different measurements. J Metz is working from the perspective of standards readiness and product availability; Stephen and Greg are working from the perspective of market adoption. Neither is a good indicator alone of enterprise readiness; rather, both need to be taken together.

* Interested in a bit more detail on how VNX volumes work? Check out [this article](http://blog.virtualtacit.com/post/11063152344/emc-vnx-volumes-a-lay-of-the-land) by Joe Kelly of Varrow.

* Scott Drummonds has a great series going on titled "The Flash Storage Revolution". In [part 1](http://vpivot.com/2011/10/04/the-flash-storage-revolution-part-i/), Scott discussed why flash is so important in enterprise storage today; in [part 2](http://vpivot.com/2011/10/13/the-flash-storage-revolution-part-ii/), Scott addressed the factors that companies must consider when deciding how to best use flash in their environments. I'm looking forward to part 3!

* Brandon Riley has a good couple of posts showing some differences between PowerPath/VE and Round Robin on VMAX ([part 1](http://www.virtualinsanity.com/index.php/2011/10/03/powerpath-ve-versus-round-robin-on-vmax-round-1/) and [part 2](http://www.virtualinsanity.com/index.php/2011/10/11/powerpath-ve-versus-round-robin-on-vmax-round-2/)). The differences with "out of the box" settings are quite dramatic in favor of PowerPath/VE; with some tuning, Round Robin pulls in much closer. Of course, raw performance is important, but failure behaviors are also important; it would be great if Brandon could incorporate some failure scenario behaviors into his scorecard.

* Jeramiah Dooley of VCE has a good article examining [the value of FAST VP and FAST Cache for service providers](http://vmforsp.typepad.com/vm-for-service-providers/2011/11/fast-and-fast-cache-for-the-service-provider.html). It's a good read that I'd recommend.

## Virtualization

* It seems that writing a series of articles is all the rage these days; Chris Colotti has a series going titled "vCloud Director Clone Wars" that discusses the considerations around the use of vSphere clones in vCloud Director environments. Have a look at the series: [part 1](http://www.chriscolotti.us/vmware/vcloud/gotcha-vcloud-director-clone-wars-part-1-overview/), [part 2](http://www.chriscolotti.us/vmware/vcloud/gotcha-vcloud-director-clone-wars-part-2-deep-dive/), and [part 3](http://www.chriscolotti.us/vmware/vcloud/vcloud-director-clone-wars-part-3-design-considerations/).

* Want to use PXE with VMs under VMware Fusion? [This post](http://fritshoogland.wordpress.com/2009/12/17/pxe-boot-in-vmware-fusion-using-host-only-adapter/) shows you how.

* Interested in running Hyper-V under ESXi 5? It's possible; [this VMware Communities document](http://communities.vmware.com/docs/DOC-8970/) provides some information. I'd also recommend having a look [here](http://www.vladan.fr/vmware-workstation-8-how-to-run-hyper-v/) as well.

* While we are on the top of nested VMs, William Lam wrote up [how to install the VMware VSA in nested ESXi 5 host](http://www.virtuallyghetto.com/2011/09/how-to-install-vmware-vsa-in-nested.html).

* Here's another article series, this time from Itzik Reich and addressing VMware SRM 5 with EMC Symmetrix ([part 1](http://itzikr.wordpress.com/2011/10/08/vmware-srm-5-with-emc-symmetrix---whats-new-part-1/) and [part 2](http://itzikr.wordpress.com/2011/10/11/vmware-srm-5-with-emc-symmetrix---whats-new-part-2-2/)).

* Cisco UCS VM-FEX is the subject of this 3-part series from Joe Keegan at Infrastructure Adventures ([part 1](http://infrastructureadventures.com/2011/09/29/deploying-cisco-ucs-vm-fex-for-vsphere-part-1concept/), [part 2](http://infrastructureadventures.com/2011/10/09/deploying-cisco-ucs-vm-fex-for-vsphere---part-2-ucsm-config-and-vmware-integration/), and [part 3](http://infrastructureadventures.com/2011/10/15/deploying-cisco-ucs-vm-fex-for-vsphere---part-3-dvs-and-guest-configuration/)).

* More nesting madness: running Virtual PC inside Hyper-V? Ben Armstrong discusses [the need for MAC spoofing](http://blogs.msdn.com/b/virtual_pc_guy/archive/2011/10/11/an-unusual-reason-to-enable-mac-spoofing.aspx) in that scenario.

* Want an opportunity to win a $50 gift card? Go supply your VDI read/write IOPS data statistics over [at Andre's site](http://myvirtualcloud.net/?p=2352).

* It's no secret that I've been discussing stretched clusters for quite some time (as far back as last September with [this presentation][1], and then again [here][2] and [here][3]), so it's great to see other people in the virtualization community talking about the subject as well. Duncan posted [an article focusing on failure scenarios](http://www.yellow-bricks.com/2011/10/05/vsphere-5-0-ha-and-metro-stretched-cluster-solutions/) and Chad Sakac posted [an article on the new stretched cluster HCL category](http://virtualgeek.typepad.com/virtual_geek/2011/10/new-vmware-hcl-category-vsphere-metro-stretched-cluster.html). This December at the Brisbane and Melbourne VMUG events, I'll be presenting some new content on stretched clusters, so stay tuned for that.

I guess it's time to wrap up now. Thanks for reading, and feel free to share any useful or pertinent links in the comments below.

[1]: {{< relref "2010-09-30-stretched-cluster-presentation-from-denver-vmug.md" >}}
[2]: {{< relref "2011-05-10-stretched-clustering-presentation.md" >}}
[3]: {{< relref "2011-10-03-updated-stretched-cluster-presentation.md" >}}
