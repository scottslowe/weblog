---
author: slowe
categories: Liveblog
comments: true
date: 2011-07-11T13:35:28Z
slug: brkcrs-2031-multilayer-campus-architectures-and-design-principles
tags:
- Cisco
- CiscoLive2011
- Networking
title: 'BRKCRS-2031: Multilayer Campus Architectures and Design Principles'
url: /2011/07/11/brkcrs-2031-multilayer-campus-architectures-and-design-principles/
wordpress_id: 2324
---

This session is BRKCRS-2031, Multilayer Campus Architectures and Design Principles. The presenter is Mark Montaez, a Principal Engineer in the Office of the CTO at Cisco.

The session focuses on the basics of building a rock-solid campus design, providing a foundation for more advanced campus design sessions available later this week at Cisco Live.

One goal of campus design is to drive down convergence, ideally to the 200 ms goal. The human ear will notice drops in 150 to 200 ms, and video is even worse.

The agenda will start with multilayer campus design principles, then move on to foundation services, campus design best practices, IP telephony considerations, QoS considerations, and security considerations. All in all, it looks like quite a beefy and information-packed session.

A typical multilayer campus design uses a modular, structured approach using the core-distribution-access architecture. The idea is to avoid organic growth and ensure deterministic behavior. If a node fails, you want to be able to easily determine the impact of that failure.

Some key advantages of a multilayer campus design include:

* Offers hierarchy; each layer has a specific role

* Promote small failure domains (limits the impact of a failure)

* It's modular, using a "building block" approach

* Promotes load balancing and redundancy

* Incorporates balance of Layer 2 and Layer 3 and leverages the strengths of both layers

* Promotes deterministic traffic patterns

* Easy to grow, understand, and troubleshoot

Focusing on the access layer. Some key thoughts about the access layer include:

* The access layer is a feature-rich layer

* The access layer provides services like QoS, trust boundary, broadcast suppression, IGMP snooping

* The access layer supplies security-related features like DHCP snooping, 802.1x, port security, DAI, and IPSG. (Some of these acronyms I'm not familiar with---can anyone fill me in?)

Now moving on to the distribution layer, this is where we provide policy, convergence, QoS, and high availability.

Above the distribution layer is the core, which provides additional aggregation. The core provides flexibility and room for growth. Is a core layer necessary? It depends on your network. If two layers provides enough growth for your environment, two layers (access and distribution) are just fine. However, if the environment needs to grow, then a core layer provides more flexibility. Even using a full mesh with a two-layer design (access and fully-meshed distribution) is OK, if that satisfies the needs of the network.

Generally speaking, when you have three or more distribution blocks, you need a core layer. Without a core layer, as you move beyond three distribution blocks with a full mesh it becomes increasingly complex, increasingly difficult to troubleshoot, and less deterministic in the event of link or node failure.

The use of distribution blocks gives flexibility in what "types" of access layers you can deploy: routed access, VSS, WAN, data center (broad Layer 2), Internet, etc. This gives lots of flexiblity at the access layer while remaining standardized at the distribution and core layers.

The help eliminate Spanning Tree issues, you can use a "Layer 2 access" model where VLAN = subnet = single access switch. VLANs do not span access layer switches. In many cases, though, it's necessary to span VLANs across access layer switches (think vMotion).

There are also "Routed Access" and VSS access layer designs. In the Routed Access design, you push Layer 3 all the way down to the access layer switches. This commits you to the "VLAN = subnet = single access layer switch" mentality and you can't span some VLANs if that's necessary. Mark (the presenter) describes routed access as "brittle" because it doesn't offer the flexibility to span VLANs where necessary. (A comment was raised that there are ways to bridge Layer 2 across Layer 3 to address this limitation.) The VSS access layer design helps eliminate loops and provide the logical hub-and-spoke design that encourages deterministic behavior.

Use point-to-point interconnections to help ensure that physical link failure or node failure is immediately detected. Also, use fibre optic cabling because it provides more flexibility in detecting failures (fibre optic can detect the failure more quickly and help us achieve the 200 ms convergence time we're shooting for).

Another way to help improve convergence times is to use fibre optic interconnections; this provides faster detection of link failure. Using physical interfaces for routing instead of SVIs can also provide faster convergence and quicker routing updates.

The presenter also made mention of the recommended version of Spanning Tree to run, but I was unable to capture what he said. I think it's in the presentation; I'll review it later and see if I can update this blog post.

How do we harden Spanning Tree and make it behave the way we want? Some things to do:

* Place the root where you want it

* Enable RootGuard (on downlinks), LoopGuard (on uplinks and Layer 2 link between distribution switches), UplinkFast, UDLD

* Only end station traffic should be seen on an edge port (use BPDU Guard, RootGuard, PortFast, and port-security). Port security, in particular, can help against switches coming onto the network that don't provide BPDUs and therefore BPDU Guard wouldn't protect us.

Even when using VSS solutions (VSS and vPC), you still need to configure STP to protect against loops during those times when the link aggregates aren't fully configured.

There was a question asked about FlexLink, but I'm not familiar with FlexLink. Anyone have resources for more information on FlexLink?

The presenter now moves on to Routing (Layer 3) considerations and best practices. This is primarily focused on routing between the distribution and core layers.

One recommendation is "build triangles not squares". With a "triangle" architecture, link/node failure does not require routing protocol convergence; a square architecture requires routing protocol convergence which increases convergence time (remember we are shooting for a 200 ms convergence target).

With regard to routing protocols, limit OSPF/EIGRP peering through the access layer. Use the `passive-interface` command to limit unnecessary peering. Using `passive-interface default` (and then unblocking the desired interfaces) is a less labor-intensive way of handling it.

You should also summarize routes at the distribution list. If I'm understanding the presenter correctly, this helps limits EIGRP queries and OSPF LSA propagation. This helps ensure more deterministic behavior in the event of link/node failure. If you do use summarization at the distribution layer, you must provide a link between the distribution layer switches.

By default, the current load balancing options will generally provide good utilization of ECMP (Equal Cost Multipath) links. However, you might need to tune the load balancing algorithms. Be careful to avoid CEF polarization by using the same algorithm at all the layers of the network; this can result in underutilized interfaces. To avoid CEF polarization, set different algorithms at each layer.

VTP version 3 can provide the necessary hardening functionality to address issues with VTP v1/2 (where VLANs accidentally get killed due to a new server taking over VTP server role and overwriting VLAN configuration network-wide). Generally don't need VTP in the "VLAN=subnet=access switch" sort of configuration.

Disabling DTP negotiation can help improve link up convergence time (using the `switchport nonegotiate` command). There's a trade-off here, naturally.

UDLD is Unidirectional Link Detection and protects against one-way communication. UDLD is primarily used on fibre optic links and helps ensure proper communication between switches. UDLD Aggressive sets both sides to err-disabled; UDLD Normal only err-disables the side where unidirectional communication was detected.

Turning PAgP off on CatOS switches and ensuring matching link aggregation configuration on both sides can help improve link restoration convergence times.

The general rule of thumb is 20:1 oversubscription from access to distribution and 4:1 oversubscription from distribution to core.

Pay close attention to the interaction between ECMP links and EtherChannel/link aggregation and how links will/will not be utilized in the event of a link failure.

The presenter was still going strong (there had been lots of questions and that threw off the schedule), but I had to go ahead and leave to make the next session.
