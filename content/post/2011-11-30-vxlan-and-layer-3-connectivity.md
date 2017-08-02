---
author: slowe
categories: Explanation
comments: true
date: 2011-11-30T11:35:47Z
slug: vxlan-and-layer-3-connectivity
tags:
- Networking
- OTV
- Virtualization
- VXLAN
title: VXLAN and Layer 3 Connectivity
url: /2011/11/30/vxlan-and-layer-3-connectivity/
wordpress_id: 2484
---

**Note:** I've posted a follow-up to this article with some corrected information. Please [read here][1].

I've been doing quite a bit of networking-related reading over the last few weeks, and VXLAN has been a key topic of this networking-related reading (along with OTV, MPLS, and OpenFlow). Since VXLAN's announcement at VMworld US 2011, there have been some pretty good articles written and published about VXLAN. Here are a few, for example:

[Digging Deeper into VXLAN, Part 1](http://blogs.cisco.com/datacenter/digging-deeper-into-vxlan/)  

[VXLAN Deep Dive, Part 2: Looking at the Options](http://blogs.cisco.com/datacenter/vxlan-deep-dive-part-2-looking-at-the-options/)  

[Digging Deeper in VXLAN, Pt 3, More FAQs](http://blogs.cisco.com/datacenter/digging-deeper-in-vxlan-pt-3-more-faqs/)  

[The Care and Feeding of VXLAN](http://codingrelic.geekhold.com/2011/09/care-and-feeding-of-vxlan.html)  

[VXLAN Part Deux](http://codingrelic.geekhold.com/2011/09/vxlan-part-deux.html)  

[VXLAN Conclusion](http://codingrelic.geekhold.com/2011/09/vxlan-conclusion.html)  

[Google+ discussions on VXLAN](https://plus.google.com/102097377740741991073/posts/2tnVCHkeVyZ?hl=en)  

[VXLAN Primer - Part 1, BORGcube Blogs](http://www.borgcube.com/blogs/2011/11/vxlan-primer-part-1/)  

However, the one thing that I haven't seen a great discussion about is the impact of VXLAN on Layer 3 connectivity. I personally have fielded a number of questions about whether VXLAN will fix Layer 3 network connectivity problems with stretched clusters. So, I thought I'd take a stab here. Networking gurus (you know who you are), feel free to straighten me out if I'm wrong.

First, let's start with a few basic things that we know about VXLAN:

* We know that VXLAN encapsulates Layer 2 frames into Layer 3 packets (using UDP).

* We know that VXLAN adds a 24-bit VXLAN Network Identifier (VNI) that allows for up to 16 million unique combinations.

* We know that VXLAN Segments are built between VXLAN Tunnel End Points (VTEPs). In the initial implementation of VXLAN, the VTEP will be the Nexus 1000V VEM on an ESXi host.

* We know that (for now) VXLAN is not understood by any physical networking devices (the transport that carries the encapsulated frames only needs to an IP-based network). (VXLAN encapsulation is a subset of OTV encapsulation, so in theory the Nexus 7000 hardware is capable of decoding VXLAN.)

With that information in mind, I'd like to use the following diagram to frame the discussion.

![](/public/img/vxlan-l3-diagram-small.png)

In the diagram, there are two ESXi hosts acting as VTEPs. Between them exist two VXLAN segments with two different VNIs (VNI 738 and VNI 864). Because VXLAN works by encapsulating Layer 2 frames into Layer 3 packets and then routing these packets between VTEPs, VXLAN accomplishes one of its primary goals: it extends Layer 2 connectivity across Layer 3 networks.

But what does that mean, exactly?

Let's look a bit more closely. The brown shape loosely represents Layer 2 connectivity within VNI 738 (a given VXLAN segment) and its associated VLAN(s). The Windows-based VM on the ESXi host on the left can communicate via Layer 2 with the Linux-based VM on the ESXi host on the right, even though those ESXi hosts reside in completely different broadcast domains separated by a Layer 3 routed network. The key phrase here, in my mind, is that **VXLAN extends Layer 2 connectivity _within_ a given VXLAN segment**.

This is _not_, however, the sort of "extending Layer 2 connectivity across Layer 3 networks" that people are expecting.

What people are expecting from this phrase is that you could migrate a VM from the ESXi host on the left to the ESXi host on the right (as indicated in the diagram by the large arrow pointing from left to right) and still have full IP connectivity.

In this case, the VM itself will be able to maintain the same IP address, and other VMs _in the same VXLAN segment_ will continue to communicate with the migrated VM without any issues. But hold on a second

We know that VXLAN allows for duplicate IP addressing schemes across different VXLAN segments (but not in the same VXLAN segment), duplicate MAC addresses across different VXLAN segments (but not in the same VXLAN segment), and duplicate VLAN IDs across different VXLAN segments (but not in the same VXLAN segment). You could, for example, use the same IP addressing scheme, same MAC addresses, and same VLAN IDs in the brown (VNI 738) and blue (VNI 864) VXLAN segments. VXLAN wouldn't care, and the VMs inside those VXLAN segments would be unaware of this duplicity.

However, what VXLAN _doesn't_ address is IP translation; that functionality is relegated to a network address translator. In this case, it's vShield Edge (VSE). So, in the instance where a VM is migrated between different Layer 3 networks, note that the only way to maintain IP connectivity from **outside** the VXLAN segment is to update the address translation tables and---here's the kicker---assign the VM a new (and different) externally-accessible IP address. That breaks IP connectivity---even with dynamic DNS updates, clients will still have cached DNS responses pointing them back to the (now inactive) old external IP address. Thus, **VXLAN breaks Layer 2/3 connectivity to other systems outside the VXLAN segment.**

This issue, by the way, would be why various networking gurus have repeatedly stated that VXLAN does not replace OTV. To fix the issue described above, you'd have to use OTV to stretch the external-to-VXLAN VLANs so that the NAT mappings could remain unchanged and the externally accessible IP address would remain the same.

Before you assume that I knocking VXLAN, let me reaffirm that I'm not. I only felt that there hadn't been a good, solid, understandable explanation of what sorts of connectivity were and were not extended/affected by VXLAN. Hopefully, this message has helped bring some clarity to the topic.

If I have misrepresented anything, presented something incorrectly, or if you have questions/clarifications, please let me know in the comments. Thanks!

**UPDATE:** As a couple of readers pointed out in the comments (thanks!), the Layer 3 connectivity isn't quite as dire as what I've described. Instead of the VM's address having to change due to a change in NAT mappings on a VSE, instead the VM's traffic will "trombone" back to the original VSE that acts as the VXLAN segment's default gateway. Again, thanks for the clarification/correction all!

[1]: {{< relref "2011-12-07-revisiting-vxlan-and-layer-3-connectivity.md" >}}
