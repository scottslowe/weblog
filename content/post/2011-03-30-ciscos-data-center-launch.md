---
author: slowe
categories: News
comments: true
date: 2011-03-30T23:56:53Z
slug: ciscos-data-center-launch
tags:
- Cisco
- FCoE
- Networking
- Nexus
- Virtualization
title: Cisco's Data Center Launch
url: /2011/03/30/ciscos-data-center-launch/
wordpress_id: 2251
---

Cisco had a pretty major product/technology launch today. I was briefed on the product launch some time ago---one of the perks of being a "leading blogger"---and so it was great to see this stuff finally come to light. I haven't had the chance to read some of the other blog posts on the topic (I know there have been several), so I apologize for any duplication.

First, let's take a quick look at the "new product" news included in the launch:

* Cisco is launching the Nexus 3000, a new member of the Nexus family. It's a high density, ultra-low latency 1RU switch that supports both 1GbE as well as 10GbE. All ports are wire rate and non-blocking. At first glance, this seemed to be a bit of a conflict with some of the Catalyst switches (like the 4948, perhaps?), but I think the distinction here is the low latency target (approximately 1 usec port to port). It also makes sense if customers are interested in standardizing on switches running NX-OS instead of a mix of NX-OS and IOS.

* Cisco is also launching new Nexus 5500 series switches, the Nexus 5548UP and Nexus 5596UP. These are very much like the other members of the Nexus 5500 family, except that the "UP" designation indicates that they will support "Unified Ports". Unified Ports are ports that can support either Fibre Channel (supporting up to 8Gbps FC), Fibre Channel over Ethernet (at 10Gbps), or Ethernet at 1Gbps. You will need to swap SFPs (for now, at least) and reconfigure the port, and you must cycle/reset the module in which the ports reside in order to switch personality. (It's my understanding that future improvements will require only cycling the specific port being switched.) Personally, I think the Unified Port functionality is extremely useful and I'm glad to see Cisco deliver it.

* Cisco is also announced a new C-series rack mount server, the C260 M2. This is a dual-socket server running Intel Westmere CPUs, supporting up to 64 DIMM slots (using Cisco's Memory Expansion technology) and supporting up to 16 HDDs or SSDs. It's a pretty beefy server. From a virtualization perspective, the expanded memory footprint is pretty useful, but I'm not so sure that the 16 drive bays is much of a selling point. I'd love to hear others' thoughts on that matter.

* Also in the new product lineup are some new Catalyst 6500 modules, an ACE-30 module, an ASA service module, and a ES-40 40Gb DCI module for high-speed data center interconnects.

* Finally, Cisco is launching an FCoE module for the MDS, which will allow customers to integrate FCoE into their networks while preserving their investment in the MDS infrastructure. On its own, this is interesting, but when coupled with other announcements...well, read on for more details.

However, the launch isn't just about new products; if it were, it would be pretty boring, to be honest. The news included in the launch about the expansion of existing technologies is, in my mind, far more significant:

* First, Cisco is stepping away from the "VN-Tag" moniker and focusing instead of the IEEE standardization of that functionality, a la 802.1Qbh. That's a good move, in my opinion; they're also going to be adding 802.1Qbg support in the future. Not such a good move (again, in my opinion) is the rebranding effort around VN-Tag and VN-Link, which are now going to be called Adapter FEX and VM-FEX. I don't know that this new naming convention is going to be any less confusing or any more clear than the previous strategy. It is a bit more technically accurate, at least, since---if you read [my post on network interface virtualization][1]---you know that the Cisco Virtual Interface Controller (VIC), aka "Palo", is really just a fabric extender (FEX) in a mezzanine card form factor. So, calling this Adapter FEX is a bit more accurate. VM-FEX....well, this still isn't so clear. If any Cisco folks want to help clear things up, feel free to jump in by posting a comment to this post.

* Cisco is adding FCoE support to the Nexus 7000 via the F1 line cards. This brings FCoE support directly to the Nexus 7000, which is a move that many expected. Along with the FCoE support, Cisco is also introducing the idea of a new type of virtual device context (VDC), the storage VDC. A storage VDC will process all FCoE traffic passing through the switch, in a completely separate fashion from LAN traffic (which would run in a separate LAN VDC). At least initially, Cisco will require the use of a storage VDC to use FCoE with the Nexus 7000; that might change over time. As with the introduction of the FCoE module for the MDS 9500, this news is interesting, but is really only useful in conjunction with other announcements. Specifically...(drum roll please)

* In perhaps the biggest announcement of the event, Cisco is now supporting full multi-hop FCoE topologies. As I understand it, there will be a new release of NX-OS that will support the creation of VE_ports and enable multi-hop FCoE topologies (up to 7 hops supported). This functionality will exist not only on the Nexus 5000/5500, but also on the Nexus 7000 and on the FCoE module of the MDS 9500. As far as I know, this also means that any Nexus 2000 series fabric extenders that support FCoE will also now support multi-hop FCoE via their upstream Nexus switch. Cisco is going to require dedicated FCoE links between switches, at least at first, and as I mentioned earlier will require the use of a storage VDC on the Nexus 7000. I believe that this is probably the most significant announcement being made, and when taken together with other FCoE-related announcements like the FCoE module on the MDS 9500 and FCoE support on the Nexus 7000, opens up lots of new possibilities for FCoE and Unified Fabric in the data center. I, for one, am really excited to dive even deeper into the design considerations around multi-hop FCoE and Unified Fabric. Any network gearheads interested in doing a brain dump on multi-hop FCoE for me?

* Equally important in the long-term but (apparently) not making a big impact immediately is LISP (Location ID/Separation Protocol). I've been talking LISP with Cisco for a while now (as I suspect many of you have), so it's good to see the official announcement. Lots of people confuse the purpose/role of LISP when compared to OTV; they are both equally important but in very different ways. Further, LISP does not replace OTV (or vice versa). I'll probably try my hand at a separate blog post to specifically discuss these two technologies and how the plan is for them to work hand-in-hand with each other for workload mobility. For now, it should suffice to say that OTV addresses Layer 2 connectivity between data centers while LISP helps the rest of the network more efficiently understand and adapt to the Layer 2 connectivity between data centers. Both are necessary.

There were a few other tidbits---like the ability to run up to 6 Nexus 1000V VSMs on the Nexus 1010, or support for the Virtual Security Gateway (VSG) on the Nexus 1010--but this covers the bulk of the information.

All in all, it's exciting to see some of these technologies coming to light, and I'm really excited to see how the data center is going to evolve over the next couple of years. It's a great time to be in this industry, especially if you're a glutton for learning new technologies like me!

As always, I invite and encourage discussion, so if I've left something important out or if I've misrepresented something, please speak up in the comments (courteously and with full disclosure, where applicable). Thanks!

**UPDATE:** In response to information gathered via Twitter (I'm [@scott_lowe](http://twitter.com/scott_lowe)), I've updated the information on the 5584UP/5596UP and Unified Ports functionality. Thanks for the great discussion!

[1]: {{< relref "2010-03-16-understanding-network-interface-virtualization.md" >}}
