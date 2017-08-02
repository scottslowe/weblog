---
author: slowe
categories: Musing
comments: true
date: 2011-08-08T09:00:00Z
slug: thinking-out-loud-what-if-vsphere-supported-fabricpath
tags:
- Cisco
- Networking
- Virtualization
- VMware
- vSphere
- ToL
title: 'Thinking Out Loud: What if vSphere Supported FabricPath?'
url: /2011/08/08/thinking-out-loud-what-if-vsphere-supported-fabricpath/
wordpress_id: 2366
---

Since attending Cisco Live 2011 in Las Vegas earlier this year (see my [summary list of blog posts][1]), my mind has been swirling with ideas about how various technologies might work (or not work) together. In the first of a series of "Thinking Out Loud" posts in which I'll attempt to explore---and spur discussion regarding---how certain networking technologies might integrate with virtualization technologies, I'd like to explore this question: "What if vSphere had FabricPath support?"

&lt;aside&gt;By the way, if you're not familiar with the "Thinking Out Loud" posts, the key point is simply to stimulate discussion and encourage knowledge sharing.&lt;/aside&gt;

If you're unfamiliar with FabricPath, you might find my [session blog of a FabricPath session][2] at Cisco Live helpful to bring you up to speed. Also, while this post focuses on potential FabricPath integration, I think many of the same points would apply to TRILL and potentially SPB.

When I first started considering this idea, a few things came to mind:

1. The first thing that came to mind was that the quickest, most "natural" way of bringing FabricPath support to vSphere would be via the Nexus 1000V. While I'm sure that the NX-OS code base between the Nexus 1000V and the Nexus 7000 (where FabricPath is available) is dramatically different, it's still closer than trying to add FabricPath support directly to vSphere.

2. The second thought was that, intuitively, FabricPath would bring value to vSphere. After all, FabricPath is about eliminating STP and increasing the effective bandwidth of your network, right?

It's this second thought I'd like to drill down on in this post.

When I first started considering what benefits, if any, FabricPath support in vSphere might bring, I thought that FabricPath would really bring some value. After all, what does FabricPath bring to the datacenter network? Multiple L2 uplinks between switches, low latency any-to-any switching, and equal cost multipathing, to name a few things. Surely these would be of great benefit to vSphere, wouldn't they? That's what I thought...until I created a diagram.

Consider this diagram of a "regular" vSphere network topology:

![Non-FP-aware network](/public/img/non-fp-aware-network.png)

This is fairly straightforward stuff, found in many data centers today. What would bringing FabricPath into this network, all the way down to the ESXi hosts, give us? Consider this diagram:

![FP-aware network 1](/public/img/fp-aware-network-01.png)

We've replaced the upstream switches with FabricPath-aware switches and put in our fictional FP-aware Nexus 1000V, but what does it change? From what I can tell, not much changes. Consider these points:

* In both cases, each Nexus 1000V has two uplinks and has the ability to actively use both uplinks. The only difference the presence of FabricPath would make, as far as I can tell, is in the selection of which uplink to use.

* In both cases, host-to-host (or VM-to-VM) traffic still has to travel through the upstream switches. The presence of FabricPath awareness on the vSphere hosts doesn't change this.

That second point, in my mind, deserves some additional thought. FabricPath enables multiple, active L2 links between switches, but in both of the topologies shown above the traffic has to travel through the upstream switches. In fact, the only way to change the travel patterns would be to add extra host-to-host links, like this:

![FP-aware network 2](/public/img/fp-aware-network-02.png)

OK, if these extra host-to-host links were present, then the presence of FabricPath at the ESXi host layer might make a difference. VM-to-VM traffic could then just hop across without going through the upstream switches. All is good, right?

Not so fast! I'm not a networking guru, but I see some potential problems with this:

* This approach isn't scalable. You'd have to have host-to-host links between every ESXi host, which means for _N_ hosts you'll need _(N-1)_ uplinks. That would limit the scalability of "fabric-connected" vSphere hosts since there just isn't enough room to add that many networking ports (nor it is very cost effective).

* Does adding host-to-host links fundamentally change the nature of the virtual switch? The way virtual switches (or edge virtual bridges, if you prefer) operate today is predicated upon certain assumptions; would these assumptions need to change? I'm not sure about this point yet; I'm still processing the possibilities.

* What does this topology _really_ buy an organization? Most data center switches have pretty low L2 switching latencies (and getting lower all the time). Would a host-to-host link really get us any better performance? I somehow don't think so.

In the end, it seems to me that there is (currently) very little value in bringing FabricPath (or TRILL or SPB) support all the way down to the virtualization hosts. However, I'd love to hear what you think. Am I wrong in some of my conclusions? My understanding of FabricPath (and related technologies) is still evolving, so please let me know if something I've said is incorrect. Speak up in the comments!

[1]: {{< relref "2011-07-15-summary-of-cisco-live-2011-blog-posts.md" >}}
[2]: {{< relref "2011-07-12-brkdct-2081-cisco-fabricpath-technology-and-design.md" >}}
