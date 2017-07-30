---
author: slowe
categories: Liveblog
comments: true
date: 2009-09-02T20:24:55Z
slug: ta2384-deploying-the-nexus-1000v
tags:
- Cisco
- Networking
- Nexus
- Virtualization
- VMware
- VMworld2009
- vSphere
title: TA2384 - Deploying the Nexus 1000V
url: /2009/09/02/ta2384-deploying-the-nexus-1000v/
wordpress_id: 1641
---

There is no Internet connectivity in this session, so I'll have to publish this after the session has concluded.

The Cisco Nexus 1000V is, of course, a Layer 2 distributed virtual switch for VMware vSphere built on Cisco NX-OS (the same operating system that drives the physical Nexus switches). It's compatible with all switching platforms, meaning that it doesn't require physical Nexus switches upstream in order to work. The Nexus 1000V brings policy-based VM connectivity, network and security property mobility, and a non-disruptive operational model.

The Nexus 1000V has two components: the Virtual Supervisor Module (VSM). Interestingly enough, the slide shows that the VSM can be a virtual or physical instance of NX-OS; there has been no formal announcement of which I know that has discussed using a physical instance of NX-OS as the VSM for the Nexus 1000V. The second component is the Virtual Ethernet Module (VEM), which is a per-host switching module that resides on each ESX/ESXi host. A VSM can support up to 64 VEMs in a distributed logical switch model, meaning that all VEMs are centrally managed by the VSM. Each VEM appears as a remote line card to the VSM.

The VEM is deployed using vCenter Update Manager (VUM) and supports both ESX and ESXi. The Nexus 1000V supports both 1Gbps and 10Gbps Ethernet uplinks and works with all types of servers (everything on the HCL) and upstream switches.

The Nexus 1000V supports a feature called virtual port channel host mode (vPC-HM). This feature allows the Nexus 1000V to use two uplinks (NICs in the server) connected to two different physical switches and treat them as a single logical uplink. This does not require any upstream switch support. Multiple instances of vPC-HM can be used; for example, you could use four Gigabit Ethernet uplinks, two to each physical switches, could be used to create two different vPC-HM uplinks for redundancy and separation of traffic.

For upstream switches that support VSS or VBS, you can configure the Nexus 1000V to use all uplinks as a single logical uplink. This requires upstream switch support but provides more bandwidth across all upstream switches. Of course, users can also create multiple port channels to upstream switches for traffic separation. There are lots of flexiblity in how the Nexus 1000V can be connected to the existing network infrastructure.

These network designs can be extrapolated to six NICs (uplinks), eight NICs, and more.

One interesting statement from the presenter was that Layer 8 (the Human layer) can create more problems than Layers 1 through 7.

Next, the presenter went through the use and configuration of the Cisco Nexus 1000V in DMZ environments. Key features for this use case include private VLANs (private VLANs can span both physical and virtual systems). Network professionals can also use access-conrol lists (ACLs) and remote port mirroring (ERSPAN) improve visibility and control over the virtual networking environment.

At this point, I left the session because it was clear that this session was more about educating users on the features of the Nexus 1000V and not about best practices on how to deploy the Nexus 1000V.
