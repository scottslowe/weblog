---
author: slowe
categories: Explanation
comments: true
date: 2010-03-21T16:29:46Z
slug: how-niv-and-fcoe-play-together
tags:
- Cisco
- FCoE
- Networking
- Virtualization
title: How NIV and FCoE Play Together
url: /2010/03/21/how-niv-and-fcoe-play-together/
wordpress_id: 1868
---

Last year, I wrote a piece about [multi-hop Fibre Channel over Ethernet (FCoE)][1] and some of the various reasons why---at the time---multi-hop FCoE was not a practical reality. Some things have changed since I last discussed multi-hop FCoE, and today I'd like to take a quick look at the interaction between network interface virtualization (NIV) and FCoE and see how this plays into multi-hop FCoE.

If you haven't already read my recent article on [understanding NIV][2], go read it now. Likewise, if you haven't read the [multi-hop FCoE article][1], you should go read it now too.  Believe me, the rest of this article will make a lot more sense if you do.

Done now? Good. Let's get started.

From my article on multi-hop FCoE, I identified two key roadblocks to multi-hop FCoE support:

1. First, there was a lack of widespread support for FCoE Initialization Protocol (FIP). FIP allows an FCoE initiator to be separated from a Fibre Channel forwarder (FCF) by one or more IEEE DCB-capable switches. This has since been addressed by updates to NX-OS (the code that runs on Cisco's Nexus 5000 switches) and by Generation 2 converged network adapters (CNAs); both of these components now have FIP support included.

2. Second, there was no defined standard for creating the FCoE equivalent of trunking E_ports (which I believe are referred to as VE_ports). VE_ports are necessary to link multiple FCFs together, much in the same way you would use an inter-switch link (ISL) in traditional Fibre Channel storage area networks (SANs). To my knowledge, this issue has not yet been addressed.

At first glance, NIV doesn't seem to help with either of these roadblocks. When you take a deeper look, though, you'll see that NIV can actually serve as a workaround for both problems. Here's how.

In NIV, recall that you have both an interface virtualization (IV)-capable bridge (or switch) as well as one or more interface virtualizers (IVs). (Remember that IVs are also referred to as fabric extenders.) Network interface cards (NICs) connect to ports on the IVs, and the IVs uplink to the IV-capable bridge. Even though the IV-capable bridge and the IVs are physically separate devices, they _appear as a single device._ Even though there is a connection---typically an Ethernet connection---between the IVs and the IV-capable bridge, it appears and functions as a single device.

With this in mind, then, I'll ask this question: what _is_ a multi-hop topology? If a multi-hop topology is multiple physical devices connected over an Ethernet uplink, then multi-hop FCoE is possible today with an FCoE-enabled IV-capable bridge and one or more FCoE-enabled IVs. In fact, this is the topology that Cisco uses in its Unified Computing System (UCS): a pair of FCoE-enabled IV-capable bridges (the UCS 6100XP fabric interconnects) connected to one or more FCoE-enabled IVs (the I/O Modules in the back of the chassis).

Applying this line of thinking to our roadblocks above, we see that the use of NIV allows for greater port densities; greater port density is one of the primary reasons why users would want FCoE initiators separated from an FCF by an IEEE DCB-capable switch. In addition to leveraging FIP (and eventually leveraging the IEEE DCB standards once they are finalized), you can build the same sort of topology using an IV-capable bridge and one or more IVs.

Similarly, using NIV as a way of connecting multiple devices together eliminates the need to chain multiple FCFs together; this, in turn, eliminates the need for the FCoE equivalent of ISLs and the need to create VE_ports between the FCFs. So NIV helps to address the second roadblock as well.

Of course, NIV isn't the only way the industry is going to address the need for multi-hop FCoE. Further---to my knowledge, at least---NIV is a Cisco-only approach. As FCoE continues to mature and the IEEE DCB standards are finalized and ratified, then organizations can leverage a standards-based approach to building more complex FCoE topologies than are currently possible today.

Courteous comments, corrections, and thoughts (with full vendor disclosure, please) are welcome below.

[1]: {{< relref "2009-08-11-why-no-multi-hop-fcoe.md" >}}
[2]: {{< relref "2010-03-16-understanding-network-interface-virtualization.md" >}}
