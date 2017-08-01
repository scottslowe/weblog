---
author: slowe
categories: Information
comments: true
date: 2009-07-27T19:25:28Z
slug: ucs-training-day-1-wrap-up
tags:
- Cisco
- FCoE
- Hardware
- Networking
- UCS
- Virtualization
title: UCS Training, Day 1 Wrap-Up
url: /2009/07/27/ucs-training-day-1-wrap-up/
wordpress_id: 1485
---

The first day of Cisco UCS training here in San Jose, CA, has just wrapped up. The class is providing some useful information so far, but I'm eager to delve deeper. I really hope the material deepens as the course progresses.

Aside from the [northbound FCoE issue][1] I posted about earlier today, there wasn't anything too terribly surprising discussed so far. I was able to have a few questions answered. For example, the half-width blades don't use Cisco's advanced memory technologies and, therefore, will suffer from the same drop in memory transaction speed (MTS) as DIMM slots are populated---just like any other vendors' Xeon 5500-based servers. I had kind of expected that Cisco would address that as a further point of differentiation from other vendors.

A few other points that came out of today's discussion that might be useful to readers included:

* Although the Cisco guys mentioned the use of Solid State Disks (SSDs) for the B-series blades, there's no orderable option for SSDs yet.

* Customers must buy RAM and disks for the B-series blades (and I would assume the C-series rack mount servers) from Cisco. There will be no support from TAC otherwise. I suspect this is an issue that will get squished pretty quickly; other server vendors have tried this approach and run afoul of US anti-trust regulations, at least with regard to memory. Remember Compaq telling customers they could only buy Compaq memory, or their warranty was void? I see something similar here.

* When connecting the IO module to the fabric interconnect, you can use 1 uplink, 2 uplinks, or 4 uplinks---but you can't use 3 uplinks. (File this under "strange limitations".)

* The number of virtual NICs (vNICs) or virtual HBAs (vHBAs) that can be presented from a Palo adapter is directly dependent upon the number of uplinks from the IO module in the blade chassis to the fabric interconnect. The stated maximum for Palo is 128 adapters, I believe; in order to get that many virtual adapters, you'd have to have multiple uplinks from the chassis to the fabric interconnects. I don't have exact figures yet, but I'll see if I can dig anything up.

I guess that's about it for now; I'll post more throughout the week. Thanks for reading, and keep the good comments coming!

[1]: {{< relref "2009-07-27-potential-ucs-issue-northbound-fcoe-connectivity.md" >}}
