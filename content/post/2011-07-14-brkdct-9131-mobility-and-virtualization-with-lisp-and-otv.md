---
author: slowe
categories: Liveblog
comments: true
date: 2011-07-14T19:11:03Z
slug: brkdct-9131-mobility-and-virtualization-with-lisp-and-otv
tags:
- Cisco
- CiscoLive2011
- Networking
- OTV
- Virtualization
title: 'BRKDCT-9131: Mobility and Virtualization with LISP and OTV'
url: /2011/07/14/brkdct-9131-mobility-and-virtualization-with-lisp-and-otv/
wordpress_id: 2346
---

This is BRKDCT-9131, Mobility and Virtualization in the Data Center with LISP and OTV. This is one of the last, if not the last, session at Cisco Live 2011.

The presenter, Victor Moreno, spends a few minutes at first talking about distributed data center clouds, the goals, and the challenges of building them. Some of the challenges or considerations include:

* L2 domain elasticity (think FabricPath and LAN extensions)

* Fabric consolidation (think Unified Fabric, VDCs)

* Storage elasticity (think SAN extensions)

* IP localization (Route optimization and route portability)

* VM awareness (think VN-Link)

Victor next discusses why LAN extensions are necessary. From a virtualization perspective, it's because a live migration requires for the IP address of the VM to remain the same. He also discusses some non-routable L2 traffic that certain applications, but to me this seems far less likely/important than maintaining IP addresses.

There are different ways of tackling LAN extensions; the [session blog from BRKDCT-3060][1] has more information on the various methods. This session is clearly slanted toward OTV, as the discussion of L2 VPNs focuses on the complexity of that solution as opposed to the benefits (like extremely fast convergence).

The next section reviews the MAC-in-IP encapsulation mechanism that OTV uses to transport L2 frames across a routed transport. Victor also reviewed how the OTV control plane (which uses IS-IS) proactively advertises MAC reachability information, so that all OTV edge devices already know what MAC addresses are reachable via the overlay.

Regardless of the method used to perform the LAN extension, this interferes with ingress routing. Subnet usually implies location, but with LAN extension this mechanism is obscured---the network doesn't know which side of the extension hosts the device to which we are communicating. This is the fundamental issue addressed by LISP.

LISP is Location Identity Separation Protocol. The idea is to separate the use of the IP address as a means of making routing decisions from the use of the IP address as a means of indicating location. We want to "split" these purposes apart. By splitting these apart, we can allow workloads to move yet still make efficient routing decisions.

So how does LISP operate? Consider this walkthrough:

1. The source endpoint performs a DNS lookup to find the destination.

2. Traffic is remote, so traffic is sent to the branch router.

3. The branch router doesn't know how to get to the destination's specific address, but it is LISP-enabled so it performs a LISP lookup to find a locator address.

4. The LISP mapping database informs the branch router how to get to the one (or more) available addresses that can get it to the destination. The LISP mapping database can return priority and weight as part of this lookup, to help with traffic engineering/shaping.

5. The branch router performs an IP-in-IP encapsulation and transmits the data out the appropriate interface based on standard IP routing decisions.

6. The receiving LISP-enabled router receives the packet, decapsulates the packet, and forwards the packet to the final destination.

To support routers that are not LISP-enabled, you use proxy tunnel routers. Proxy tunnel routers will encapsulate or decapsulate traffic from or to routers that are not LISP-enabled.

Some terminology of which you should be aware:

* EID: End-point identifier (host IP or prefix)

* RLOC: Routing locator (IP address of routers in the backbone)

* Tunnel router: Edge devices that perform encapsulation/decapsulation

* ETR: Egress tunnel router

* ITR: Ingress tunnel router

* PxTR: Proxy tunnel router (gateway between LISP backbone and non-LISP-aware routers)

* EID to RLOC mapping DB: Contains all the EID-to-RLOC mapping, distributed across multiple map servers (MS)

LISP resolution (determining the appropriate ETR by an ITR) is performed by the MS, which forwards requests to ETRs that have registered with the MS as authoritative for a particular prefix. The ITR then caches the mapping for future use.

Future revisions of the LISP standard will add extra security to prefix registrations, to avoid the malicious introduction of prefix registrations (I believe it's referred to as LISP-SEC).

LISP enables on-demand routing. Rather than requiring that all routers maintain a full routing table, LISP routers will determine LISP prefix mappings on an as-needed basis.

Some potential use cases for LISP:

* IP portability

* Ingress traffic engineering without BGP

* IPv6 transition support (6-over-4, 4-over-6, etc.)

* Multitenancy and VPNs

* VM mobility

Victor now walks through some scenarios concerning the use of LISP with VM mobility use cases. One interesting note: using LISP to provide IP portability means that I could use SRM to failover to a DR site but _not_ have to perform IP customization---LISP would handle the IP routing side of the house. That's a pretty cool combination.

The session is still going, but I have to get back to the show floor and finish tearing down the EMC booth.

[1]: {{< relref "2011-07-14-brkdct-3060-deployment-considerations-with-interconnecting-dcs.md" >}}
