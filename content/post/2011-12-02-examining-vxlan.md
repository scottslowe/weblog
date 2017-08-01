---
author: slowe
categories: Explanation
comments: true
date: 2011-12-02T09:00:00Z
slug: examining-vxlan
tags:
- Networking
- vCloud
- Virtualization
- VXLAN
title: Examining VXLAN
url: /2011/12/02/examining-vxlan/
wordpress_id: 2488
---

It's taken me far too long to write this post, that's for sure. Since the announcement of VXLAN at VMworld earlier in the year, I've been searching for additional information on these questions: "What is VXLAN? How does it fit into the broader networking landscape? Why did we need a new standard?" I talked to Cisco, I attended [a VMworld session about networking futures][1], I talked to some of the authors of the [IETF draft on VXLAN](http://tools.ietf.org/html/draft-mahalingam-dutt-dcops-vxlan-00), I read (most of) the VXLAN draft, and I studied some existing protocols that one might think could have been put to use. I think I'm finally ready to try to address these questions.

**What is VXLAN?**

The answer to this question is taken directly from the IETF draft (the emphasis is mine):

>This document describes Virtual eXtensible Local Area Network (VXLAN), which is used to **address the need for overlay networks within virtualized data centers accommodating multiple tenants.**

I think it's important to keep this purpose in mind. While it's a bit simplistic to state it this way, VXLAN is---essentially---a proposed standards-based replacement for the proprietary MAC-in-MAC encapsulation that is currently used in vCloud Director. Instead of using MAC-in-MAC encapsulation, VXLAN uses MAC-in-IP encapsulation, with multicast groups to handle MAC learning and unique UDP source ports to help with load balancing across multiple links. Yes, that's a bit of a simplification, but I think it gets the main point across.

**How does VXLAN fit into the broader networking landscape?**

Trying to answer this question is what has occupied the majority of the time it's taken to write this post. You can't explain how VXLAN fits into the broader networking landscape without having a minimal understanding, at least, of what the rest of the networking landscape looks like. I had to dig in a bit deeper to MPLS, OTV, FabricPath/TRILL, and other standards/emerging standards. I'm sure that I've still omitted some technologies that should have been included, and I know that there are still (so much) more to learn about the technologies I did include.

Based on the information I was able to gather, the answer to this second question really builds on the answer to the first question. VXLAN only really addresses a few fundamental concerns:

* A shortage of VLAN address space (the theoretical limit is 4094 VLANs, with many switches supporting fewer than that)

* An inability to support multi-tenancy (both from a scale perspective as well as a separation perspective)

* Problems with Layer 2 connectivity across disparate virtual data centers

VXLAN addresses these concerns in this way:

* It adds a 24-bit VXLAN Network Identifier (VNI), expanding the realm of potentially unique identifiers to just shy of 17 million (16.7 million). This addresses any scale-based concerns of multitenancy.

* It wraps Layer 2 frames in Layer 3 packets. This addresses the other part of any multitenancy concerns (VXLAN hides duplicate MAC addresses, duplicate IP addresses, and duplicate VLAN IDs found in separate VNIs). This also addresses the Layer 2 connectivity issues between disparate virtual data centers.

And that's really about it. It doesn't address Layer 2 multipathing/STP, it doesn't address Layer 2 connectivity in the physical world (layer 2 connectivity is only preserved at the virtualization level), and it doesn't address Layer 3 routing issues created by stretched VLANs and VM mobility designs. Which brings us to our third question

**Why did we need a new standard?**

This answer builds on the previous two answers. Once you have a clear understanding of what VXLAN was designed to do, and how VXLAN fits into the rest of the networking protocols, then this answer is pretty easy:

* If you've been reading my articles, you know already that [VXLAN doesn't preserve all forms of Layer 3 connectivity][2]. Because it doesn't, you still need protocols like OTV to address Layer 2/3 connectivity at the physical level.

* Because you still need protocols like OTV to achieve VM mobility (for the time being, at least), you're still going to need protocols like LISP to fix funny routing issues being caused by IP addresses from the same subnet existing in multiple locations at the same time.

* Because VXLAN doesn't address Layer 2 multipathing concerns, you still need protocols like TRILL and technologies like FabricPath.

* Because using MPLS---which, by the way, would also address the 3 concerns VXLAN addresses---would require MPLS-enabled/MPLS-aware equipment throughout the data center, that would make an MPLS-based solution difficult for many enterprises to adopt. Using an IP encapsulation scheme means that existing physical networking equipment doesn't have to change. (Although it might change---to add VXLAN support---at some point in the future.)

I was not a fan of VMware (apparently) driving the creation of an entirely new networking standard. However, as I dug into this, I began to see that while other solutions almost addressed these concerns, none of them were a really good fit. Yes, using MPLS probably would have worked. Using GRE might have worked (take [NVGRE](http://tools.ietf.org/html/draft-sridharan-virtualization-nvgre-00), for example, but that's also a proposed new protocol). To really address the concerns head-on, though, required a solution that was written/created expressly for that purpose, and that's VXLAN. It's just important, though, to really understand what VXLAN _is_ as well as what VXLAN _isn't_. Otherwise, you'll find yourself trying to fit VXLAN to a solution for which it really wasn't intended---which, by the way, was why VXLAN was created in the first place.

Comments, corrections, and clarifications are always welcome!

[1]: {{< relref "2011-08-31-vsp2117-virtualization-aware-datacenter-networks.md" >}}
[2]: {{< relref "2011-11-30-vxlan-and-layer-3-connectivity.md" >}}
