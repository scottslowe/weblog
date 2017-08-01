---
author: slowe
categories: Rant
comments: true
date: 2013-04-16T09:25:04Z
slug: network-overlays-vs-network-virtualization
tags:
- Networking
- Virtualization
- VXLAN
title: Network Overlays vs. Network Virtualization
url: /2013/04/16/network-overlays-vs-network-virtualization/
wordpress_id: 3140
---

Dan Wendlandt said something today in the NVP deep dive session (liveblog of the session [here][1]) that really crystallized something for me. I thought perhaps I might be the only one that was seeing a trend, but Dan's comment leads me to believe there are others seeing this trend as well. Here's the quote, taken from my liveblog of the session:

>It is important to note, as Dan does, that a tunneling protocol alone is not network virtualization.

There's a lot of buzz in the industry about network virtualization and network overlays, and often those terms are used interchangeably. People talk about the need for multi-tenancy and address space isolation, point to network overlays like VXLAN, NVGRE, and STT as the answer to all our problems, and in so doing they (inadvertently) conflate network overlays with network virtualization. Network overlays and network virtualization aren't the same thing, and people that use them interchangeably probably don't fully understand what's involved.

&lt;aside&gt;By the way, if you're not familiar with the idea of network overlays, I'd recommend reading [this][2], [this][3], and [this][4] to get you started. There's plenty more out there, but those three articles will at least prime the pump, I think.&lt;/aside&gt;

Network overlays are great for address space isolation (for example, isolating duplicate MAC addresses, duplicate IP addresses, or duplicate VLAN IDs). As such, network overlays can be an important part of network virtualization. You need more than a network overlay, though, to have network virtualization; you also need virtualized network services (like NAT, firewalls, ACLs, QoS, routing, and the like) and you need a control plane (else how would you coordinate the various pieces within the network virtualization solution?). The overlay protocol is just one piece of the puzzle, so using "network overlay" interchangeably with "network virtualization" is incorrect.

As always, I welcome the input of those more educated/knowledgeable than I am. If you're a networking expert (or a virtual networking expert), feel free to speak up in the comments and correct my misunderstanding or misconceptions (please disclose vendor affiliations). I'm always open to deepening my knowledge---and helping others with their understanding and knowledge along the way.

[1]: {{< relref "2013-04-15-openstack-summit-2013-nicira-nvp-deep-dive.md" >}}
[2]: {{< relref "2011-12-02-examining-vxlan.md" >}}
[3]: {{< relref "2011-12-07-revisiting-vxlan-and-layer-3-connectivity.md" >}}
[4]: {{< relref "2012-03-12-some-thoughts-and-questions-about-stt.md" >}}
