---
author: slowe
categories: Liveblog
comments: true
date: 2011-07-14T12:00:38Z
slug: brkdct-3060-deployment-considerations-with-interconnecting-dcs
tags:
- Cisco
- CiscoLive2011
- Networking
- OTV
title: 'BRKDCT-3060: Deployment Considerations with Interconnecting DCs'
url: /2011/07/14/brkdct-3060-deployment-considerations-with-interconnecting-dcs/
wordpress_id: 2342
---

This is BRKDCT-3060, Deployment Considerations with Interconnecting Data Centers, presented by Patrice Bellagamba (Distinguished SE) and Max Ardica (Solution Architect). Given that my role in virtualization and storage puts me squarely in the middle of these sorts of designs, I'm really looking forward to this session and gaining some valuable knowledge out of it.

The primary objectives of the session are to identify the business requirements driving DCI (Data Center Interconnect) deployments; understanding the functional components of the Cisco DCI solution; and get a full knowledge of Cisco LAN extension technologies and associated considerations. The session does NOT consider path optimization (ACE/GSS, LISP), storage extension considerations, and workload mobility application-specific considerations.

So what are some of the business drivers and solutions? There are several:

* Business continuity (disaster recovery, HA framework)

* Operational cost containment (DC maintenance or migration)

* Business resource optimization (disaster avoidance, workload mobility)

* Cloud services (inter-cloud networking, _X_aaS)

It is important, when extending VLANs between sites, to preserve STP domain isolation and storm control in order to keep fault domain as small as reasonably possible. A key part of a DCI model is path optimization, but that is not something that will be covered in this session.

In a VLAN extension environment, there are 5 different types of VLANs you might deploy in your data center:

* Type T0: Limited to a single access layer device

* Type T1: Extended within an aggregation block/pod

* Type T2: Extended between aggregation blocks/pods within a single data center site

* Type T3: Extended between aggregation blocks/pods as part of "twin DC sites" (usually connected via dark fiber; think metro/synchronous distances)

* Type T4: Extended between aggregation blocks/pods as part of remote DC sites (think geo/asynchronous distances)

Note that these classifications are just for understanding the different ways VLANs are extended; this is not a configuration class or type.

There are three basic types of VLAN extensions: Ethernet-based, IP-based, or MPLS-based.

The Ethernet-based solution leverages dark fiber/DWDM connections between DCs. In this configuration, make sure to filter BPDUs on one site and enable broadcast storm control. You can leverage technologies like vPC or VSS to avoid loops and take advantage of redundant links between the DC sites. If you use vPC, you will need separate (dedicated) Layer 3 links for inter-DC routing.

There was a question about the distances supported for an Ethernet solution. According to Max, it's less about distance and more about the dark fiber/DWDM/direct fibre connectivity requirements. Up to 60km is probably a reasonable distance for this solution.

The Ethernet-based solution is a viable solution, but Max prefers OTV.

OTV is Overlay Transport Virtualization and is something I've discussed on my site before. The OTV edge device (ED) is the device that performs the OTV encapsulation (MAC-in-IP encapsulation). The internal interfaces are the interfaces on the ED that face the site. The join interface is the interface of the ED that faces the core. The overlay interface is a logical entity that encapsulates the traffic.

Some considerations:

* Join interfaces and internal interfaces are only supported on M1 modules.

* The join interface can't be an SVI or loopback interface, but it can be a port-channel interface or a routed physical interface (or sub-interface).

There were some other considerations but I couldn't capture them in time.

OTV encapsulation adds 42 bytes to the packet IP MTU size. Be sure to keep this consideration in mind; ensure that you have the appropriate MTU size support end-to-end. The OTV control plane uses IS-IS to learn MAC address reachability information from remote OTV peers. In the current release of NX-OS, OTV requires multicast support in the transport network connecting the sites. NX-OS 5.2 will add unicast (Adjacency Server mode) support.

The adjacency server (or daemon) is "just an OTV edge device" that advertises the IP of each ED to all other EDs (in something called the OTV Neighbor List, or the oNL). All subsequent communications happen directly between EDs without going through the adjacency server/daemon.

By default OTV will isolate STP domains, which is an important part of any DCI solution. OTV also does not flood unknown unicast frames. OTV relies on the OTV control plane in order to ensure MAC address reachability information is in the MAC address table of edge OTV ED.

Some current limitations:

* Up to 3 sites for OTV

* Up to 128 extended VLANs

* 12K MAC addresses

In NX-OS 5.2 these values increase to 6 sites, 256 VLANs, and 16K MAC addresses.

Max now shifts into a discussion of OTV deployment considerations. The first topic is where to place the OTV ED.

One option is to deploy the OTV ED in the core. This is an easy deployment in brownfield scenarios. In this case, the L2-L3 boundary remains at aggregation. You could have core devices perform both L3 and OTV functions, or you could use separate core devices, or you could use Nexus 7000 VDCs (see [my VDC session blog][1] for more information).

However, Max prefers deploying OTV at the aggregation layer. This preserves "pure" L3 functionality at the core and is consistent with OTV's role as an "overlay" functionality.

Traffic on a Nexus 7000 belonging to a given VLAN can either be routed or extended, but not both. You must use dual VDCs to do both routing and OTV extension on the same Nexus 7000 physical device (either at the core or the aggregation layer).

Deploying OTV at the aggregation layer is recommended for greenfield deployments, but it does require Nexus 7000 at the aggregation layer.

A third option is to deploy OTV over dark fiber connections, much like the Ethernet-based solution described earlier. In this scenario you would not need separate L3 links as with the Ethernet-based solution. Because you are doing both routing and OTV extension at the same time, this solution would require dedicated OTV VDCs.

Max next covered the various scenarios of how you can actually connect the OTV VDC and L2/L3 VDCs. There are advantages and disadvantages to the various approaches: the simple appliance model uses fewer links but introduces other challenges; the most resilient model uses more links but provides faster convergence and better traffic flow.

Keep in mind that OTV is a pure IP encapsulation technology that does not include any L4 information (only L3 information wrapped around a MAC address), so IP hashing technologies (port-channel load balancing or ECMP hashing) won't spread traffic across multiple links. In a two-site scenario, only one of two links will be used---always. In a three-site design, two links can be used (depends on the hashing). Future hardware updates will allow flow-based load balancing (F2 or F3 module, perhaps).

Native OTV support on F-series modules is targeted for a future hardware release. Neither F1 nor F2 modules support OTV currently.

In the event of various failure scenarios, only failures that cause an AED (Authoritative Edge Device) re-election cause extended outages for convergence. In NX-OS 5.2, convergence will be between 5 and 30 seconds for an AED re-election. Future releases will bring those values down. Failures that do not cause AED re-election will converge in sub-second timeframes.

Max still recommends configuring storm control (using the `storm-control broadcast` command) on the internal interface if you are concerned about broadcast storms with OTV.

Now Patrice takes over to discuss MPLS-based solutions (refer to [my MPLS session blog][2] for more information). There are three MPLS-based solutions: EoMPLS, A-VPLS, and H-VPLS.

EoMPLS is Ethernet-over-MPLS. To do this, you would use the `xconnect` command with MPLS encapsulation to create a point-to-point Ethernet over MPLS connection. In fact, a `show cdp neighbor` will show the remote port over the MPLS network.

This use of MPLS allows us to take advantage of all of MPLS' features, like traffic engineering, for the traffic moving over this EoMPLS connection.

You can use LACP/vPC with multiple EoMPLS links.

You can also use MPLS over IP (using a GRE tunnel) and then create the EoMPLS pseudowires inside the GRE tunnels. (I think---this part got pretty complicated.) Because you are using GRE tunneling, you can apply an IPsec encryption profile against that tunnel to provide encryption for the EoMPLS traffic.

EoMPLS is point-to-point, so what about point-to-multipoint? VPLS can address this. With multiple devices connected across an MPLS core, each VSI (Virtual Switch Instance) will create multiple EoMPLS pseudowires to create a full mesh between all other VSIs.

There are considerations on attaching edge devices in this sort of configuration; best practices would be to make the MPLS-connected devices the STP root bridge (or avoid STP through vPC or equivalent). Patrice walks through a number of different deployment considerations and discussions of where to place the devices, but the focus of the discussion seemed to be more on VSS than A-VPLS. I'll need to go back and review this information again in order to get a better understanding of the material.

Patrice now moves on to a discussion of H-VPLS.

H-VPLS is more suited for service providers (SPs) or SP-like enterprises. You might see this when interconnecting provider DCs, connecting enterprise DCs to provider DCs, or in high-end enterprise DCI scenarios.

ICCP is Inter-Chassis Communication Protocol that is in draft status with the IETF. I'm not clear on how this is related to vPC, vPC+ (since Patrice was talking about FabricPath), or other multi-chassis link aggregation solutions. I think the connection to H-VPLS is that you might use ICCP in connecting Layer 2 devices to the edge devices that will then be participating in the MPLS network.

Some terms:

* mLACP Multi-Chassis Link Aggregation Protocol

* MC-LAG: Multi-Chassis Link Aggregation Group

* DHD: Dual-homed device (customer edge)

* DHN: Dual-home network (customer edge)

Ah, here's the information that helps me decipher the flow of the session. A-VPLS is only for certain devices (Catalyst 6500); H-VPLS is for high-end devices (7600, ASR-9K).

At this point I had to leave for a meeting so I wrapped up the session blog. Patrice was in the summary section of the presentation so no new information was going to be shared.

[1]: {{< relref "2011-07-13-brkdct-2121-virtual-device-context-design-and-implementation-considerations.md" >}}
[2]: {{< relref "2011-07-11-brkmpl-1101-introduction-to-mpls.md" >}}
