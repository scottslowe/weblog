---
author: slowe
categories: Liveblog
comments: true
date: 2011-07-11T16:17:26Z
slug: brkmpl-1101-introduction-to-mpls
tags:
- Cisco
- CiscoLive2011
- Networking
title: 'BRKMPL-1101: Introduction to MPLS'
url: /2011/07/11/brkmpl-1101-introduction-to-mpls/
wordpress_id: 2328
---

This session is BRKMPL-1101, Introduction to MPLS (Multiprotocol Label Switching). The presenter is Hari Rakotoranto, a product manager for MPLS at Cisco. I'm looking forward to this session as I've always heard a lot about MPLS, but haven't ever had the opportunity to really learn more about it. Hopefully this session won't be too far over my head.

MPLS is used for a variety of purposes; two of the most common are Layer 3 VPNs and Layer 2 VPNs. But why MPLS? What are the business drivers for MPLS? One of the first use cases was SP/carrier networks, where they needed to consolidate networks, provide multiple L2/L3 services, support increasingly stringent SLAs, and handle the increasing scale and complexity of IP-based networks. From the enterprise space, the trend picked up later and was driven by multi-site configurations and the need for network segmentation. MPLS originated in the mid-1990s and evolved through approximately 2004 when MPLS OAM became available.

MPLS is built on labels. Every packet is stamped with a label, then MPLS will switch that packet through the network based on the label. There is an MPLS forwarding plane (where labeled packets are switched instead of routed) and an MPLS control/signaling plane (where MPLS leverages existing IP-based control protocols and extensions).

Within the enterprise space, there are two general types of MPLS deployments:

* The enterprise is subscribed to an MPLS-based network from a provider

* The enterprise has deployed MPLS in it's own network

Continuing with the enterprise space, there are three major reasons for deploying/using MPLS:

1. Network segmentation (network virtualization, distribution application virtualization)

2. Network realignment/migration (consolidation of multiple networks)

3. Network optimization (full-mesh and hub-and-spoke deployments, Traffic Engineering [TE] for bandwidth protection)

The presenter now shifts gears to the technology components of MPLS.

The core of MPLS is the MPLS signaling and forwarding components. Within the MPLS network, there is a Label Switched Path (LSP) from one end to the other end based on a label. There are a number of different terms applied in an MPLS network:

* PE (Provider Edge): This is an edge router that adds labels (on ingress) or removes lables (on egress).

* P (Provider) router: This is a label-switching router or a core router.

* CE (Customer Edge)

The MPLS label itself is a 32-bit structure. The first part of the label is the actual MPLS label, which occupies 20 bits. Then you have the EXP/COS bits for QoS handling, and then you have the S bit; the S bit represents the bottom of the stack. (The S bit facilitates multiple layers of MPLS labels.) The label wraps up with an 8 bit TTL. The MPLS label is usually placed just after the transport header (for example, just after the MAC header for Ethernet).

Some additional terms to understand:

* Label imposition: Occurs at the PE at ingress; classify and label packets

* Label swapping or switching: Occurs at the P router; forwards packets based on label and indicates service class and destination

* Label disposition: Occurs at a PE on egress; remove label and forward packets

The Forwarding Equivalence Class (FEC) is the mechanism to map Layer 2/3 packets onto an LSP by the ingress PE router. There are a variety of FEC mappings possible. (The presenter advanced the slide before I could capture some of that information.)

Label Distribution Protocol (LDP) handles exchanging label information between and among MPLS nodes. There is a push operation (used on the ingress PE node to know which label to use for a given FEC), swap operation (occurs on the core P node), pop operation (occurs at egress PE node to inform node about FEC mapping). LDP is a superset of Cisco-specific TDP, and you can also use BGP with some extensions as well. As an MPLS control plane protocol, it is L3 based (runs over IP) and uses a specific set of TCP ports and protocols to communicate MPLS label information.

The presenter next walked through the general steps involved in MPLS forwarding.

MPLS supports various IGPs: EIGRP, IS-IS, OSPF on core facing and core links. RSVP and LDP are supported on core and/or core-facing links. MP-iBGP runs on edge routers.

I mentioned the S bit (Bottom of Stack) bit earlier; one of the real superpowers of MPLS is label stacking. This allows organizations to stack labels for QoS, security, traffic engineering, segmentation, etc.

One of the key drivers for MPLS is VPN: both Layer 3 and Layer 2 VPNs. The connection to the MPLS network is handled by the PE-CE link; this is the connection between the PE (for ingress/egress) and the CE. Once the traffic enters the MPLS network, then all the advantages of MPLS (via labels and label stacking) become available.

Options for MPLS VPNs include:

* Layer 2 VPNs (point-to-point and multi-point): Routing is always CE-CE; no SP involvement

* Layer 3 VPNs

Focusing a bit more on L3 VPNs. In this case, the CE has a peering link with the PE and there is IP routing/forwarding across the PE-CE link. In this case, the MPLS VPN is part of the customer's IP routing domain. MPLS VPNs enable full-mesh, hub-and-spoke, and hybrid connectivity among connected CE sites.

Components of an L3 MPLS VPN:

* PE-CE link

* MPLS L3 VPN control plane

* MPLS L3 VPN forwarding plane

VRFs (Virtual Routing and Forwarding instances) are created for each customer VPN on the PE router. Each VRF is associated with one or more customer interfaces, has its own routing table (RIB) and forwarding table (CEF) and has its own instance for PE-CE configured routing protocols.

MP-iBGP is Multi-Protocol BGP extensions; this is for supporting non-IP protocols over BGP. Typically BGP RR (Route Reflector) to improve scalability.

MPLS uses a Route Distinguisher (RD) along with a VPNv4 address to help ensure that all customer routes are unique across the MPLS network.

Hari (the presenter) next walked through several use cases for MPLS VPNs (traffic separation, network integration, shared access to services, and simplified hub-and-spoke designs).

Next, the presenter dove into L2 VPNs with MPLS. A new term was introduced: AToM (Any Transport over MPLS). L2 VPNs enable the transport of any Layer 2 traffic over an MPLS network by building PE-to-PE pseudowires. Other new terms are introduced as well: Attachment Circuits (ACs), which are the PE-CE links; VC, for virtual circuit; and PWES, for Pseudo Wire End Services.

Use cases for MPLS L2 VPNs include L2 network interconnect (data center interconnect?) and Virtual Private LAN Service (VPLS) to emulate an IEEE Ethernet bridge. The presenter went into some additional detail about VPLS, VPLS components, and VPLS operations. Some key thoughts I was able to capture: VPLS uses a targeted LDP session between PEs to establish the VC and pass traffic. Multiple VSIs (Virtual Switching Instances) exist to pass traffic. You might use VPLS to create a "Metro Ethernet" backbone connect multiple sites together in a full mesh network.

After wrapping up L2 VPNs, Hari moves "back" down the MPLS stack to discuss MPLS QoS. MPLS mostly leverages existing IP QoS architecture based on DiffServ model. MPLS uses the EXP bits instead of the IP ToS field. Most MPLS providers supply 3 to 5 service classes. The ingress PE router maps DSCP values into EXP bits. (I'm guessing this would be part of the FEC mappings, so that different DSCP values would translate into different FEC mappings. Is that accurate?) There are different modes of MPLS QoS that do or do not preserve the DSCP values (Uniform, Pipe, and Short Pipe).

The next topic is MPLS TE (Traffic Engineering). You might use MPLS TE when you want to optimize your network traffic. This could be due to any number of reasons: congestion, link/node failures, or new services coming online. Generally speaking, IP networks will forward based on a "shortest path" algorithm. Because this typically doesn't include bandwidth constraints, this model can create some congested links. MPLS TE can help address this issue. MPLS TE uses ISIS-TE or OSPF-TE (to distribute link information), CSPF (to calculate the shortest path), and RSVP-TE (to setup up the path). MPLS TE can also be used to provide Fast Reroute (FRR); this provides a pre-provisioned backup tunnel (path) in the event the primary tunnel (path) has a failure. FRR can provide faster failover times (~50 ms, depending on topology and other factors).

The session wraps up with a discussion of MPLS management. The discussion centers around SNMP MIBs and OAM.
