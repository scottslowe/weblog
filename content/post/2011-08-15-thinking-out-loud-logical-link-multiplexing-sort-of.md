---
author: slowe
categories: Musing
comments: true
date: 2011-08-15T09:00:00Z
slug: thinking-out-loud-logical-link-multiplexing-sort-of
tags:
- Networking
- Virtualization
- ToL
title: 'Thinking Out Loud: Logical Link Multiplexing (Sort Of)'
url: /2011/08/15/thinking-out-loud-logical-link-multiplexing-sort-of/
wordpress_id: 2370
---

Over the last few days I've been listening to some episodes of the excellent Packet Pushers Podcast, trying to learn about such things as Shortest Path Bridging (SPB) and 802.1Qbg. It's this second item--802.1Qbg---that this blog post is about (indirectly).

If you're unfamiliar with 802.1Qbg, I highly recommend Ivan Pepelnjak's articles on the topic:

[Edge Virtual Bridging (EVB; 802.1Qbg) Eases VLAN Configuration Pains](http://blog.ioshints.info/2011/05/edge-virtual-bridging-evb-8021qbg-eases.html)  

[EVB (802.1Qbg)  The S Component](http://blog.ioshints.info/2011/05/evb-8021qbg-s-component.html)

This second article, in particular, contained a statement which really prompted this "Thinking Out Loud" post. In his discussion of EVB and the S component, Ivan states:

>Logical link multiplexing might seem a solution in search of a problem until you discover that VMware-related design documents usually recommend using 6 to 10 NICs per server---an approach that either wastes switch ports or is hard to implement with blade servers mezzanine cards (due to limited number of backplane connections).

I'm quite familiar with the design recommendations that led vSphere architects and administrators to build environments with 6, 8, 10, or 12 NICs in their servers---I designed and implemented my fair share of them. If that's what's driving logical link multiplexing, then it seems to me that we are constraining our next-generation networks and next-generation data centers with design criteria from the past. This was the idea behind [the tweet I posted](http://twitter.com/scott_lowe/status/102300694786220032) very early in the morning:

>It would seem to me that logical link multiplexing is the result of imposing the designs of the past on top of the technology of the future.

Why did we need 6, 8, or 10 NICs in our servers in the past? Because we only had Gigabit Ethernet networks, and we needed more bandwidth than a couple of Gigabit Ethernet links provided. Add to that recommendations saying that management traffic needed to be separate from vMotion traffic which needed to be separate from VM traffic, when in reality a fair part of those recommendations stemmed from even earlier versions of ESX (anyone remember the dedicated Service Console NIC from ESX 2.x?). More than a few of these criteria don't apply any more. Why are we still imposing those thought processes on environments leveraging dual or quad 10 Gigabit Ethernet links? Why are we driving technologies that allow us to emulate old ways of doing things on new platforms and new technologies?
