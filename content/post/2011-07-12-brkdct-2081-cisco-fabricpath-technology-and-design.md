---
author: slowe
categories: Liveblog
comments: true
date: 2011-07-12T16:26:17Z
slug: brkdct-2081-cisco-fabricpath-technology-and-design
tags:
- Cisco
- CiscoLive2011
- Networking
title: 'BRKDCT-2081: Cisco FabricPath Technology and Design'
url: /2011/07/12/brkdct-2081-cisco-fabricpath-technology-and-design/
wordpress_id: 2336
---

This is BRKDCT-2081, Cisco FabricPath Technology and Design. FabricPath is (to my understanding) Cisco's implementation of TRILL, and as a key data center technology this is something I'm very interested in learning. Tim Stevenson is the presenter.

FabricPath is a technology to drive Layer 2 in the data center. Why Layer 2 in the data center? Because customers are asking for it! The presenter casually mentions "applications that require Layer 2 adjacency," and of course my first thought is the VLAN spanning requirements for vMotion. This is not to spark a Layer 2 vs. Layer 3 design, but rather to focus on those specific parts of the network where Layer 2 adjacencies are required. In the typical multilayer network (see my post on [the campus multilayer design session][1]), this would typically be an access layer "pod."

To extend L2 to greater areas, you _could_ extend the L2 boundary further up the layers of the network and use Spanning Tree Protocol to prevent loops. Obviously, this creates underutilized links and larger failure domains, so this wouldn't exactly be the ideal solution. Other potential problems might include:

* STP doesn't offer any load balancing

* STP convergence is disruptive

* Tree topologies introduce sub-optimal paths

* MAC address tables don't scale

* Flooding impacts the whole network

The goal of FabricPath is to combine the best aspects of L2 with the best aspects of L3. That means easy configuration, "plug and play," multipathing (ECMP), fast convergence, and easy scalability.

So what is FabricPath? FabricPath turns a network into a "fabric." It makes a group of switches act like a single switch. There's no STP inside the fabric. (In this regard, it sounds a lot like making a group of FabricPath-capable switches act like a group of fabric extenders. I need to think a bit more about this.)

Key goals of FabricPath:

* Optimal, low-latency switching using shortest path any-to-any (the ingress switch identifies the output port across the fabric; no STP inside)

* High bandwidth, high resiliency with Equal Cost Multi-Pathing (up to 256 links between any 2 devices in the fabric; traffic is redistributed across remaining links in case of failure, thus providing fast convergence; sounds like this uses port channels?)

* Scalable using "conversational learning" (per-port MAC address table only needs to learn peers that are reached across the fabric; provides much greater MAC address scalability)

* Layer 2 integration using vPC+ (allows you to connect non-fabric devices to the fabric using IEEE standard port channels to prevent loops and avoid the use of STP; vPC+ is just vPC extended for FabricPath)

* Edge device integration to allow hosts to leverage multiple L3 default gateways (hosts see a single default gateway, but the fabric provides multiple simultaneously active default gateways; maximum is currently two devices as default gateways)

* Layer 3 integration (an arbitrary number of routed interfaces can be created and attached L3 devices can peer with them; the hardware is capable of handling millions of routes;  SVIs are fully supported)

Now we move on to a more technical overview of FabricPath. FabricPath uses a new control plane using L2 IS-IS. IS-IS assigns addresses to all FabricPath switches automatically and computes the shortest pair-wise path. There is full support for equal-cost paths between any FabricPath switch pairs. Cisco uses an "extended" version of IS-IS.

From a data plane perspective, MAC address/switch ID associations are maintained at the edge of the FabricPath network. FabricPath uses switch IDs to compute paths via IS-IS; "traditional" Layer 2 uses MAC addresses to forward frames.

Here's some terminology:

* CE edge ports: Edge ports connected to classical Ethernet devices. This is the "outer boundary" of the fabric. CE edge ports participate in STP, maintains MAC address tables, and send/receives traffic in standard Ethernet format.

* FP core ports: This an interface connected to another FabricPath device. These ports form IS-IS adjacencies, no STP, no BPDUs, and send/receive only FabricPath frames.

In the event of an unknown unicast, FabricPath will perform an unknown unicast flood (like classical Ethernet/transparent bridging). The flood will still be encapsulated in a FabricPath header, but it will be sent to all switches participating in the fabric. However, other switches will not learn the MAC address unless the end device is attached to that switch (this goes back to the conversational learning approach).

An important note regarding hardware: FabricPath does require F1 series I/O modules on the Nexus 7000 series switch and NX-OS 5.1. M series I/O modules cannot switch FabricPath traffic. Both FP core and CE edge ports must be on an F1 series I/O module. FabricPath uses a new VLAN mode (mode fabricpath in the VLAN configuration context), and FabricPath VLANs can only be enabled on F1 series modules.

Now the presenter provides some additional detail on vPC+. vPC+ allows dual-homed connections from edge ports into a FabricPath domain with active/active forwarding. The only requirement from the device being connected is the ability to form port-channel interfaces. (Does this mean we could use a vSwitch with "Route based on IP hash"?) Configuration is almost identical to vPC. Key requirement: peer-link and all vPC+ connections must be made to ports on an F1 series I/O module.

There was a brief mention of trying to do L3 peering across the vPC/vPC+ peer link. To my understanding, vPC+ behaves like vPC in that regard.

Now for a comparison of FabricPath to TRILL. TRILL is an IETF standard for L2 multipathing. TRILL has moved from Draft to Proposed Standard. Cisco FabricPath hardware is also TRILL capable. The F1 I/O modules are capable of providing either FabricPath or TRILL encapsulation, but there is no software support yet. The mention of FP encapsulation vs. TRILL encapsulation leads me to believe that FabricPath and TRILL are fundamentally incompatible. This is further underscored by mention of a future software upgrade that will add a "TRILL mode" to FabricPath.

With regard to FabricPath vs. TRILL, there are a number of features that FabricPath supports that TRILL currently does not: vPC+ or its equivalent; FHRP active/active; multiple topologies; conversational learning; and the types of inter-switch links supported (TRILL might have a slight advantage here in the support of shared inter-switch links).

FabricPath supports the idea of multiple topologies, which is a way of constraining a VLAN to a subset of the links within a FabricPath fabric. This is not currently in the TRILL standard. This provides some VLAN pruning, traffic engineering, security, etc., benefits.

Edge ports can be PVLAN ports. (But FP core ports cannot?)

Concerning STP participation, FabricPath looks like a single bridge. It currently expects to be the root bridge (CE edge ports will block if they receive better information) and it sends the same STP information on all edge ports. No BPDUs are forwarded across the fabric.

FabricPath IS-IS runs on the switch supervisor as a separate component in the control plane. The presenter gave a great overview of the FabricPath system architecture, but I was unable to capture the information quickly enough here.

FabricPath maintains a shortest (best) path to each Switch ID (recall Switch IDs are assigned by IS-IS) based on link metrics. ECMP is supported between FabricPath switches. You can use the `show fabricpath isis route` command to see the IS-IS view of the FabricPath topology. The `show fabricpath route` command shows the U2RIB's view of the routing topology.

FabricPath builds two multidestination trees to handle unknown unicast, multicast, and broadcast traffic. The multidestination trees are designed to provide loop-free access to all the switches in the fabric. They are not used for known unicast frames. The trees are build by IS-IS and the roots of each tree are elected by IS-IS. The IS-IS election is handled by "priority value" (highest System ID, highest Switch ID used in the event of a tie). Cisco does recommend explicitly setting the root via priority value so that IS-IS elects a known root for the multidestination trees.

FabricPath uses a 16 byte MAC-in-MAC encapsulation. Although the encapsulation uses a 48 bit address that looks like a MAC address, it's not. Contained in the header are the Switch ID, Sub-Switch ID, Local ID (LID), Ftag (Forwarding tag), and TTL (uses to prevent frames from circling the fabric endlessly). The Switch ID is a 12 bit value provisioned automatically by the DRAP (runs on the supervisor as part of the FabricPath control plane). The Sub-Switch ID is an 8 bit value used in vPC+ and must be unique within each vPC+ virtual switch domain. The Local ID (LID) identifies the exact port which sourced the frame or to which the frame is destined. The Ftag is a 10 bit number that identifies the FP topology (for known unicast frames) or the multidestination tree (for multidestination frames).

The presenter next spent some time reviewing the behavior of traffic moving through FabricPath. One key note I got from the discussion was that any unique conversation will always follow the same path through the fabric---there is no load balancing.

For multicast traffic, IGMP snooping is used on the edge ports. IS-IS learns multicast group membership based on the IGMP snooping data and creates "pruned trees" for each group on both multidestination trees. Data is then forwarded along the appropriate pruned tree on the multidestination tree. Pruned trees are built using GM-LSPs (I don't know what GM-LSPs are.)

The presenter moves on to some discussion of various FabricPath designs. The first example provide was simply attaching a FabricPath "access pod" to an existing multilayer network, which requires very little (if any) change to the rest of the network. He also showed an example of using FabricPath at the core to extend VLANs between cores without having to extend STP between the access layer pods. The third example is using FabricPath as a site interconnect mechanism. This does require dark fiber but supports any number of sites, isolates STP, and allows you to selectively extend VLANs between sites. (How does this play with/interact with/compete with OTV?)

At this point the session wrapped up.

[1]: {{< relref "2011-07-11-brkcrs-2031-multilayer-campus-architectures-and-design-principles.md" >}}
