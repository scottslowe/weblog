---
author: slowe
categories: Liveblog
comments: true
date: 2011-07-12T11:30:47Z
slug: brkcom-3002-network-redundancy-and-load-balancing-designs-for-ucs-blade-servers
tags:
- Cisco
- CiscoLive2011
- Networking
- UCS
- Virtualization
title: 'BRKCOM-3002: Network Redundancy and Load Balancing Designs for UCS Blade Servers'
url: /2011/07/12/brkcom-3002-network-redundancy-and-load-balancing-designs-for-ucs-blade-servers/
wordpress_id: 2334
---

This is BRKCOM-3002, Network Redundancy and Load Balancing Designs for UCS Blade Servers, presented by none other than M. Sean McGee (available as [@mseanmcgee](http://twitter.com/mseanmcgee) on Twitter; I highly recommend you follow him on Twitter if you are interested in UCS). I was really looking forward to this presentation, as I've spoken with Sean before and I know that he is a super-sharp UCS guru. Sean also blogs [here](http://www.mseanmcgee.com/).

Sean starts out the presentation by asking, "Do you know how UCS networking _thinks?_" This sets the stage for how he is going to explain how frames move through UCS, both northbound and southbound. So part of his goal today is to teach us how UCS networking thinks.

A quick outline of the presentation is:

* A review of components (fabric interconnects and extenders, converged network adapters)

* A look at network path control mechanisms

Sean spends a few minutes reviewing the high-level architecture of the UCS and some of the differentiation between UCS and other typical blade architectures. He reminds the attendees that the fabric interconnects are more than just switches; they are the management for the entire system (the fabric interconnects are where UCS Manager runs). UCS Manager offers both high-level views of UCS as well as very detailed views of UCS. Sean also spends a bit of time reviewing the idea of service profiles as an encapsulation of the server's identity and the central role that service profiles play in a UCS environment.

Over the next few minutes Sean discusses the trade-offs that come with blade servers (decreased server visibility and increased points of management) and how that plays into Cisco's approach for UCS. The use of fabric extenders is key to eliminating some of the visibility loss and reducing points of management. This leads into an extended discussion of the fabric interconnects and fabric extenders and the relationship between them.

Next, a brief overview of the CNAs available for the UCS leads to a discussion of the VIC (Palo) and the use of Cisco's FEX technology in virtualized environments. Here are the different FEX "types":

* Rack FEX: This is a typical N5K/N2K sort of deployment.

* Chassis FEX: This is what UCS uses: UCS 61xx plus fabric extenders in the blade chassis.

* Adapter FEX: This is VIC (Palo), creating virtual interfaces all the way into an operating system (any OS for which Cisco provides VIC/Palo drivers).

* VM-FEX: This is the use of VIC (Palo) plus a management link between UCS and vCenter Server to connect VMs directly to the VIC, essentially "bypassing" the hypervisor. (As a side note: this has numerous design considerations that you have to consider with regards to vSphere.)

Now Sean opens up some new stuff...

A new fabric interconnect is in the works; the Cisco 6248UP fabric interconnect. This has more ports and supports Unified Ports. The Unified Port support is very cool; that allows any of the 48 ports to be either Ethernet or Fibre Channel.

A new fabric extender is also being released; the UCS 2208XP fabric extender (goes in the chassis). The 2208XP fabric extender will offer 8 uplinks to the fabric interconnects (2x the uplinks of the 1st gen UCS fabric extender). The new fabric extender also offers more downlinks to individual servers (4x the downlinks versus previous generations). You can also use port-channeling between the fabric extender and the fabric interconnects.

Cisco is also releasing the VIC 1280, a new generation of the VIC (Palo) adapter. This offers dual 4x 10GbE uplinks for up to 80Gbps per host. The VIC 1280 can also present up to 116 interfaces to the OS (or hypervisor). The VIC 1280 can also use port-channeling up to the fabric extender, which was not supported in previous generations.

All of these components are fully interoperable with previous generation versions of the components.

Sean now switches gears to focus on the behaviors of UCS networking and "how UCS networking thinks."

The first topic is the network path control mechanisms. There are several mechanisms in place, depending on "where" in the networking stack they are applicable. For example, at the "edge" of UCS there are mechanisms like Spanning Tree Protocol versus border port pinning (affected by End Host Mode or Switch Mode on the fabric interconnects) only operates at the fabric interconnect level.

Sean spends a few minutes explaining the differences between a traditional switch and a vSwitch. For example, vSwitches don't do MAC learning and don't run STP; traditional switches don't receive broadcasts only on one port and don't use host pinning for load balancing.

Sean then uses that discussion to frame the difference between UCS fabric interconnects in Switch Mode (acting like a traditional switch) versus End Host Mode (acting like a vSwitch). End Host Mode makes the fabric interconnects "look" and "act" like a vSwitch. You can use active/active uplinks without using port channeling and the fabric interconnect uses pinning to attach servers to uplinks. Overall, it was  very good comparison.

Border port pinning is what UCS uses to load balance traffic from servers onto the fabric interconnect uplinks. Individual blades are either dynamically pinned to an uplink or statically (manually) pinned to an uplink. Traffic on the same VLAN and the same fabric interconnect is locally switched. All uplinks are active (because of End Host Mode) and there's no concern for loops and no need for port channeling. You can use port channeling for uplinks, and those port channels can be used with either dynamic or static pinning.

Some best practices for pinning:

* Use dynamic pinning unless there is a specific reason to use static pinning.

* Use port channels where possible for improved redundancy.

UCS also offers fabric port pinning (the way in which the fabric extender assigns uplinks). There are two modes: discrete mode and port channel mode. In discrete mode, you can use Fabric Failover (more info later) to assign a primary fabric path and a backup fabric path. This would allow you to modify the assignment of a server to a fabric extender uplink. You could also use NIC teaming in the OS (or hypervisor) to do the same thing and direct traffic to one port versus another port.

Port channel mode (only supported on the Gen2 components) changes all that behavior; the uplinks between the fabric extenders and the fabirc interconnects can be used by any server in the chassis. This is a nice improvement, in my opinion. Port channel mode is selected in the Chassis Discovery Policy in UCS Manager.

Port channel mode versus discrete mode is selected on a per-chassis basis; you can have different chassis with different settings. This offers some flexibility for customers who might want more specific control over traffic placement.

The Gen2 CNA (the VIC 1280) and the Gen2 fabric extender (UCS 2208XP) can also use port channeling between them (this is new).

Sean then walked the attendees though various combinations of Gen1 and Gen2 hardware with both discrete mode and port channel mode. His recommended practice (assuming you have all Gen2 hardware) is to use port channel mode between both the VIC 1280 and the fabric extender and between the fabric extender and the fabric interconnect.

Next up is a discussion of fabric failover and how it behaves and acts. For customers that are concerned about only a single CNA, you can deploy full-width blades with dual CNAs. Naturally, you configure fabric failover within a service profile. Only the Gen1 Menlo cards and Palo cards support fabric failover. Note that you can use fabric failover in conjunction with VM-FEX (pushing virtual interfaces all the way to VMs) to help control which VMs communicate over which fabric.

OS NIC teaming is also an option for directing/controlling traffic within a UCS environment. As with all other things, there are advantages and disadvantages of both approaches, so plan carefully.

Sean wraps up the session with a review of the decision process UCS follows to drive a frame northbound out of UCS, and then the same decision process when forwarding frames southbound.

All in all, this was an excellent session with lots of useful information. A fair amount of it was review for me, but still useful to attend nevertheless.
