---
author: slowe
categories: Liveblog
comments: true
date: 2014-09-11T12:26:37Z
slug: idf-2014-architecting-for-sdi-a-microserver-perspective
tags:
- Hardware
- IDF2014
- Intel
- Networking
title: 'IDF 2014: Architecting for SDI, a Microserver Perspective'
url: /2014/09/11/idf-2014-architecting-for-sdi-a-microserver-perspective/
wordpress_id: 3531
---

This is a liveblog for session DATS013, on microservers. I was running late to this session (my calendar must have been off---thought I had 15 minutes more), so I wasn't able to capture the titles or names of the speakers.

The first speaker starts out with a review of exactly what a microserver is; Intel sees microservers as a natural evolution from rack-mounted servers to blades to microservers. Key microserver technologies include: Intel Atom C2000 family of processors; Intel Xeon E5 v2 processor family; and Intel Ethernet Switch FM6000 series. Microservers share some common characteristics, such as high integrated platforms (like integrated network) and being designed for high efficiency. Efficiency might be more important than absolute performance.

Disaggregation of resources is a common platform option for microservers. (Once again this comes back to Intel's rack-scale architecture work.) This leads the speaker to talk about a Technology Delivery Vehicle (TDV) being displayed here at the show; this is essentially a proof-of-concept product that Intel built that incorporates various microserver technologies and design patterns.

Upcoming microserver technologies that Intel has announced or is working on incude:

* The Intel Xeon D, a Xeon-based SoC with integrated 10Gbs Ethernet and running in a 15-45 watt power range

* The Intel Ethernet Switch FM10000 series (a follow-on from the FM6000 series), which will offer a variety of ports for connectivity---not just Ethernet (it will support 1, 2.5, 10, 25, 40, and 100 Gb Ethernet) but also PCI Express Gen3 and connectivity to embedded Intel Ethernet controllers. In some way (the speaker is unclear) this is also aligned with Intel's silicon photonics efforts.

A new speaker (Christian?) takes the stage to talk about software-defined infrastructure (SDI), which is the vision that Intel has been talking about this week. He starts the discussion by talking about NFV and SDN, and how these efforts enable agile networks on standard high volume (SHV) servers (such as microservers). Examples of SDN/NFV workloads include wireless BTS, CRAN, MME, DSLAM, BRAS, and core routers. Some of these workloads are well suited for running on Intel platforms.

The speaker transitions to talking about "RouterBricks," a scalable soft router that was developed with involvement from Scott Shenker. The official term for this is a "switch-route-forward", or SRF. A traditional SRF architecture can be replicated with COTS hardware using multi-queue NICs and multi-core/multi-socket CPUs. However, the speaker says that a single compute node isn't capable of replacing a large traditional router, so instead we have to scale compute nodes by treating them as a "linecard" using Intel DPDK. The servers are interconnected using a mesh, ToR, or multi-stage Clos network. Workloads are scheduled across these server/linecards using Valiant Load Balancing (VLB). Of course, there are issues with packet-level load balancing and flow-level load balancing, so tradeoffs must be made one way or another.

An example SRF built using four systems, each with ten 10Gbps interfaces, is capable of sustaining 40Gbps line rate with 64 bytes, 128 bytes, 256 bytes, 512 bytes, 1024 bytes, and 1500 bytes. Testing latency and jitter using a Spirent shows that an SRF compares very favorably with an edge router, but not so well against a core router (even though everything on the SRF is software-based running on Linux). Out of order frames from the SRF were less than 0.04% in all cases.

That SRF was built using Xeon processors, but what about an SRF built using Atom processors? A single Atom core can't sustain line rate at 64 or 128 bytes per packet, but 2 cores can sustain line rate. Testing latency and jitter showed results at less than 60 microseconds and less than 0.15 microseconds, respectively.

Comparing Xeon to Atom, the speaker shows that a Xeon core can move about 4 times the number of packets compared to an Atom core. A Xeon core will also use far less memory bandwidth than an Atom core due to Xeon's support for Direct Data I/O, which copies (via DMA) data received by a NIC into the processor's cache. Atom does not support this feature.

With respect to efficiency, Xeon versus Atom presents very interesting results. Throughput per rack unit is better for the Atom (40 Gbps/RU compared to 13.3Gbps/RU), while raw throughput is far better for the Xeon. Throughput per watt, on the other hand, is slightly better for the Atom (0.46 Gbps/watt versus 0.37 Gbps/watt for Xeon).

At this point, the first presenter re-takes the stage and they open up the session for questions.
