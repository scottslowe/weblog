---
author: slowe
categories: Musing
comments: true
date: 2010-10-24T09:39:24Z
slug: netronome-1-year-later
tags:
- Hardware
- Networking
- Virtualization
title: Netronome, 1 Year Later
url: /2010/10/24/netronome-1-year-later/
wordpress_id: 2141
---

In August 2009 I [wrote about][1] a company called [Netronome](http://netronome.com/) and the network processor they were developing. Late last week I had another conversation with Netronome and was able to follow-up on last year's discussion as well as get an idea of where they might be headed.

The product in question in August 2009 was the Network Flow Processor; specifically, the NFP-3200, a 40-core processor specifically designed to provide line-rate functionality like switching, load balancing, firewalling, or Quality of Service (QoS)---all embedded in hardware and handled directly by the NFP-3200. The idea, of course, is to offload all this functionality into the NFP so as to reduce the load on host CPUs. You can get a few more details about the NFP-32xx via [Netronome's product page](http://netronome.com/pages/network-flow-processors).

At first glance, it might seem like Netronome hasn't made a great deal of progress since last year. In August 2009, Netronome only had support for open source Xen. Unfortunately, that's still the case today. However, Netronome's business is a bit different. The comparison was made to Bose: you can buy a Bose radio as an OEM part from BMW, but you can also buy a Bose radio directly from Bose. In Netronome's case, you can buy NFP functionality when it is embedded into another manufacturer's products, or you can buy Netronome's products directly. In the past year, Netronome has been primarily focusing on the former line of business (the OEM aspect). For example, Netronome's NFP is used in an intrusion prevention system (IPS) produced by Sourcefire.

Based on my conversation late last week, it would now appear that Netronome is preparing to add a bit more visibility to their second line of business: selling Netronome-branded products. One of the form factors Netronome is exploring is a PCIe-based card with the NFP embedded on the card; this will be a dual-port 10GbE card that offers hardware-based, line-rate switching and QoS. VMware vSphere support is tentatively targeted for Q2/Q3 of 2011 (although, as with all roadmap dates, that might change). The idea here is that an IT end-user could install this card into their servers and gain hardware-virtualized NIC instances and hardware-based switching functionality directly on the card. In VMware environments, this means you could leverage something like VMDirectPath to bypass the hypervisor. Astute readers will, of course, immediately point out that VMDirectPath has its own challenges (like losing vMotion in its current incarnation); Netronome will need to address those challenges in order to make their product valuable to the end-user community.

Netronome's dual-pronged business model---targeting both OEMs and end users---means that their measures of success are very different than what many readers would consider success. For Netronome to become a Tier 1 supplier of components to a major networking vendor or carrier would be a huge success, even if Netronome-branded products never take off. As for the end-user focused business, I'll reiterate today what I said a year ago after first speaking with Netronome: the real challenge for Netronome is getting the appropriate support from major vendors, including VMware, and addressing some of the challenges of the potential use cases for their product.

In the spirit of full disclosure, Netronome did not provide any consideration or compensation for this blog post; I wrote it because the technology sounded interesting and I thought it was something about which my readers might be interested in learning more.

[1]: {{< relref "2009-08-04-netronome-and-io-virtualization.md" >}}
