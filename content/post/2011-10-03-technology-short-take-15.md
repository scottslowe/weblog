---
author: slowe
categories: Information
comments: true
date: 2011-10-03T09:00:00Z
slug: technology-short-take-15
tags:
- FCoE
- LISP
- Networking
- OTV
- Storage
- Vblock
- Virtualization
- VMotion
- VPLEX
- VXLAN
title: 'Technology Short Take #15'
url: /2011/10/03/technology-short-take-15/
wordpress_id: 2434
---

Welcome to Technology Short Take #15, the latest in my irregular series of posts on various articles and links on networking, servers, storage, and virtualization---everything a growing data center engineer needs!

## Networking

My thoughts this time around are pretty heavily focused on VXLAN, which continues to get lots of attention. I talked about posting a dissection of VXLAN, but I have failed miserably; fortunately, other people smarter than me have stepped up to the plate. Here are a few VXLAN-related posts and articles I've found over the last couple of weeks:

* There is a three-part series over at [Coding Relic](http://codingrelic.geekhold.com/) that does a great job of explaining VXLAN, the components of VXLAN, and how it works. Here are the links to the series: [part 1](http://codingrelic.geekhold.com/2011/09/care-and-feeding-of-vxlan.html), [part 2](http://codingrelic.geekhold.com/2011/09/vxlan-part-deux.html), and [part 3](http://codingrelic.geekhold.com/2011/09/vxlan-conclusion.html). One note of clarification: in part 3 of the series, Denny talks about a VTEP gateway. Right now, the VTEP gateway _is_ the server itself; anytime a packet on a VXLAN-enabled network leaves the physical server to go to a different physical server, it will be VXLAN-encapsulated. It won't be decapsulated until it hits the destination VTEP (the ESXi server hosting the destination VM). If (when?) VXLAN awareness hits physical switches, then the possibility of a VTEP gateway existing outside the server exists. Personally, it kind of makes sense---to me, at least---to build VTEP gateway functionality into vShield Edge.

* Some people aren't quite so enamored with VXLAN; one such individual is Greg Ferro. I respect Greg a great deal, so it was interesting to me to read his article on [why VXLAN is "full of fail"](http://etherealmind.com/top-5-things-vxlan-fail/). Some of his comments are only slightly related to VXLAN (the rant over IEEE vs. IETF, for example), but Greg's comment about VMware building a new standard instead of "leveraging the value of networking infrastructure" echoes some of my own thoughts. I understand that VXLAN accomplishes things that existing standards apparently do not, but was a new standard really necessary?

* Omar Sultan of Cisco took the time to compile [some questions and answers about VXLAN](http://blogs.cisco.com/datacenter/more-vxlan-qa/). One thing that is made more clear---for me, at least---in Omar's post is the fact that VXLAN _doesn't_ address connectivity to the vApps from the "outside" world. While VXLAN provides a logical isolated network segment that can span multiple Layer 3 networks and allow applications to communicate with each other, VXLAN doesn't address the Layer 3 addressing that must exist outside the VXLAN tunnel. In fact, in my discussions with some of the IETF draft authors at VMworld, they indicated that VXLAN would require a NAT device or a DNS update in order to address changes in externally-accessible applications. This, by the way, is why you'll still need technologies like OTV and LISP (or their equivalents); see [this post](http://blogs.cisco.com/datacenter/digging-deeper-into-vxlan/) for more information on how VXLAN, OTV, and LISP are complementary. If I'm wrong, please feel free to correct me.

* In case you're still unclear about the key problem that VXLAN attempts to address, this quote from Ivan Pepelnjak might help (the full article is [here](http://blog.ioshints.info/2011/09/vxlan-otv-and-lisp.html)):

	>VXLAN tries to solve a very specific IaaS infrastructure problem: replace VLANs with something that might scale better. In a massive multi-tenant data center having thousands of customers, each one asking for multiple isolated IP subnets, you quickly run out of VLANs.

* Finally, you might find [this PDF](https://communities.cisco.com/docs/DOC-26426) helpful. Ignore the first 13 slides or so; they're marketing fluff, to be honest. However, the remainder of the slides have some useful information on VXLAN and how it's expected to be implemented.

## Servers

I didn't really stumble across anything strictly server hardware-related; either I'm just not plugged into the right resources (anyone want to make some recommendations?) or it was just a quiet period. I'll assume it was the former.

## Storage

* Tony Bourke at Data Center Overlords has a couple of good FCoE-related articles. First, there's this piece on [the odd pairing of Fibre Channel and Ethernet](http://datacenteroverlords.com/2011/09/14/fibre-channel-and-ethernet-the-odd-couple/) and how they are often at odds with one another. The second article was on the overall "health" of FCoE; [is it dead, or not?](http://datacenteroverlords.com/2011/09/19/fcoe-im-not-dead-arista-youll-be-stone-dead-in-a-moment/) Only time will tell.

## Virtualization

* Did you see [this post](http://vninja.net/virtualization/network-simulation-in-vmware-workstation-8/) about new network simulation functionality in VMware Workstation 8?

* Here's a good walk-through on [setting up vMotion across multiple network interfaces](http://www.vfrank.org/2011/09/16/using-multiple-network-adaptors-for-vmotion/).

* _VMware vSphere Design_ co-author Maish Saidel-Keesing has a post here on how to approximate the functionality of [netstat on ESXi](http://technodrone.blogspot.com/2011/09/netstat-for-esxi.html).

* William Lam has a "how to" on [installing the VMware VSA with running VMs](http://www.virtuallyghetto.com/2011/09/how-to-install-vmware-vsa-with-running.html).

* Fellow vSpecialist Andre Leibovici did a write-up on a proof of concept that the vSpecialists did for a customer involving [Vblock, VPLEX, and VDI](http://myvirtualcloud.net/?p=2342). This was a pretty cool use case, in my opinion, and worth having a look if you need to design a highly available environment.

* Thinking about playing with vShield 5? That's a good idea, but check [here](http://vtexan.com/2011/08/vshield-5-issues-with-virtual-vcenter/) to learn from the mistakes of others first. You'll thank me later.

* The question of defragmenting guest OS disks has come up again and again; here's [the latest take](http://blogs.vmware.com/vsphere/2011/09/should-i-defrag-my-guest-os.html) from Cormac Hogan of VMware. He makes some great points, but I suspect that this question is still far from settled.

It's time to wrap up now; I hope that you found something useful. As always, thanks for reading! Feel free to share your views or thoughts in the comments below.
