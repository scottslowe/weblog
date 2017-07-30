---
author: slowe
categories: Liveblog
comments: true
date: 2013-09-11T17:17:19Z
slug: idf-2013-virtualizing-the-network-to-enable-sdi
tags:
- IDF2013
- Microsoft
- Networking
- SDN
- Virtualization
title: 'IDF 2013: Virtualizing the Network to Enable SDI'
url: /2013/09/11/idf-2013-virtualizing-the-network-to-enable-sdi/
wordpress_id: 3290
---

This is session EDCS008, "Virtualizing the Network to Enable a Software-Defined Infrastructure (SDI)." The speakers are Brian Johnson (@thehevy on Twitter) from Intel and Jim Pinkerton from Microsoft. Brian is a Solution Architect; Jim is a Windows Server Architect. If you've ever been in one of Brian's presentations, you know he does a great job of really diving deep in some of this stuff. (Can you tell I'm a fan?)

Brian starts the session with a review of how the data center has evolved over the last 10 years or so, driven by the widespread adoption of compute virtualization, increased CPU capacity, and the adoption of 10Gb Ethernet. This naturally leads to a discussion of software-defined networking (SDN) as a means whereby the network can evolve to keep up the rapid pace of change and innovation in other areas of the data center. Why is this a big deal? Brian draws the comparison between property management and how IT is shaping:

* A rental house is pretty easy to manage. One tenant, infrequent change, long-term investments.

* An apartment means more tenants, but still relatively infrequent change.

* A hotel means lots of tenants and the ability to handle frequent change and lots of room turnover.

The connection here is VMs---we're now running lots of VMs, and the VMs change regularly. The infrastructure needs to be ready to handle this rapid pace of change.

At this point, Jim Pinkerton of Microsoft takes over to discuss how Windows Server thinks about this issue and these challenges. According to Jim, the world has moved beyond virtualization---it now needs the ability to scale and secure many workloads cost-effectively. You need greater automation, and you need to support any type of application. Jim talks about private clouds, hosting (IaaS-type services), and public clouds. He points out that MTTR (Mean Time to Repair) is a more important metric than MTBF (Mean Time Between Failures).

Driven by how the data center is evolving (the points in the previous paragraph), the network needs to be evolved:

* Deliver networking as part of a pooled, automated infrastructure

* Ensure multitenant isolation, scale, and performance

* Expand data center capacity seamlessly as per business needs

* Reduce operational complexity

Out of these design principles comes SDN, according to Pinkerton. Key attributes of SDN, according to Microsoft, are flexibility, control, and automation. At this point Pinkerton digresses into a discussion of SMB3 and its performance characteristics over 10Gb Ethernet---which, frankly, is completely unrelated to the topic of the presentation. After a few slides of discussing SMB3 with very little relevance to the rest of the discussion, Pinkerton moves back into a discussion of the virtual switch found in Windows Server 2012 R2.

Brian now takes over again, focusing on virtual switch performance and behavior. East-west traffic between VMs can hit 60-70Gbps, because it all happens inside the server. How do we maintain that traffic performance when we see east-west traffic between servers? We can deploy more interfaces, which is commonly seen. Moving to 10Gb Ethernet is another solution. Intel needed to add features to their network controllers---features like stateless offloads, virtual machine queues, and SR-IOV support---in order to drive performance for multiple 10Gb Ethernet interfaces. SR-IOV can help address some performance and utilization concerns, but this presents a problem when working with network virtualization. If you're bypassing the hypervisor, how do you get on the virtual network?

Brian leaves this question for now to talk about how network virtualization with overlays helps address some of the network provisioning concerns that exist today. He provides an example of how using overlays---he uses NVGRE, since this is a joint presentation with Microsoft---can allow tenants (customers, internal business units, etc.) to share private address spaces and eliminate many manual VLAN configuration tasks. He makes the point that network virtualization is possible without SDN, but SDN makes it much easier and simpler to manage and implement network virtualization.

One drawback of overlays is that many network interface cards (NICs) today don't "understand" the overlays, and therefore can't perform certain hardware offloads that help optimize traffic and utilization. However, Brian shows a next-gen Intel NIC that will understand network overlays and will be able to perform offloads (like LSO, RSS, and VMQ) on encapsulated traffic.

This leads Brian to a discussion of Intel Open Network Platform (ONP), which encompasses two aspects:

1. Intel ONP Switch reference design (aka "Seacliff Trail"), which leverages Intel silicon to support SDN and network Virtualization

2. Intel ONP Server reference design, which shows how to optimize virtual switching using Intel's Data Plane Development Kit (DPDK)

The Intel ONP Server reference design (sorry, can't remember the code name) actually uses Open vSwitch (OVS) as a core part of its design.

Intel ONP includes something called FlexPipe (this is part of the Intel FM6700 chipset) to enable faster innovation and quicker support for encapsulation protocols (like NVGRE, VXLAN, and whatever might come next). The Intel ONP Switch supports serving as a bridge to connect physical workloads into virtual networks that are encapsulated, and being able to do this at full line rate using 40Gbps uplinks.

At this point, Brian and Jim wrap up the session and open up for questions and answers.
