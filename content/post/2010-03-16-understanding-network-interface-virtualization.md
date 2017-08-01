---
author: slowe
categories: Education
comments: true
date: 2010-03-16T17:33:24Z
slug: understanding-network-interface-virtualization
tags:
- Cisco
- Networking
- UCS
- Virtualization
title: Understanding Network Interface Virtualization
url: /2010/03/16/understanding-network-interface-virtualization/
wordpress_id: 1862
---

In late November 2009, I published a post on understanding NPIV (N_Port ID Virtualization) and NPV (N_Port Virtualization); you can read the [full post here][1]. In that post, I described a pair of virtualization technologies---NPV and NPIV---for Fibre Channel storage area networks (SANs). This time around, I'd like to discuss a related technology on the Ethernet networking side called network interface virtualization (NIV). This technology, by the way, is currently undergoing IEEE standardization as part of the 802.1Qbh standard under the name "Bridge Port Extension".

Before I can describe NIV, I need to define a few key terms:

_IV-capable bridge:_ This is a switch that understands and is aware of interface virtualization (IV). Examples of IV-capable bridges include the Nexus 5000 and the UCS 6100XP fabric interconnects.

_Interface virtualizer:_ An interface virtualizer (IV) is a device that simply extends the reach of an IV-capable bridge. To the outside world, an IV-capable bridge and all of its IVs appear as a single bridge. Examples of IVs include the Nexus 2000 fabric extender and the I/O module (IOM) in the Cisco Unified Computing System (UCS). There are also other forms of IVs, as I'll explain later in this post.

_Link-local tag:_ Frames entering an IV from a network interface card (NIC) have a link-local tag added to them; this link-local tag denotes the source IV port. Similarly, frames entering an IV-capable bridge have a link-local tag added to them that indicates the path through the IV(s) to the destination IV port. In this way, the link-local tag supplants the MAC address as the primary method of determining to which port (or out which port) a frame should be forwarded. The link-local tag is removed from the frame when it exits an IV (headed into a NIC) or when it exits an IV-capable bridge. (This link-local tag is what Cisco refers to as VNTag.)

Now that I've defined some NIV terminology, I'd like to explain why NIV is useful. To understand the value of NIV, take a look at the progression or development of data center networking:

1. Before virtualization, bridges (or switches) were connected to a NIC (sometimes multiple NICs, but usually just one NIC) in a physical server. The relationship between physical NICs, physical switch ports, and MAC addresses is static and easy to determine (generally one NIC with one MAC address connected to one switch port). The bridge hierarchy is reasonably simple.

2. Then hardware manufacturers introduce the server blade form factor. This introduces another layer of bridges (switches) in the blade chassis themselves and creates a more complex bridge hierarchy. This more complex bridge hierarchy also means more management, as each of these bridges represents a point of management. In order to provide redundancy between the various layers of bridges multiple connections are necessary; this, in turn, necessitates the use of Spanning Tree Protocol (STP) in order to prevent bridging loops. STP creates active/passive connections, meaning the effective bandwidth of the network is reduced.

3. Along comes virtualization, which introduces the idea of multiple virtual NICs (vNICs) associated with a single physical NIC. This, in turn, introduces another layer of switching and more points of management, and the bridge hierarchy grows more complex. Because multiple MAC addresses are now associated with a single NIC connected to a single switch port, bridges must now deal with "hairpin forwarding," in which the switch must retransmit a frame out the same port on which it was received. The relationship between NICs, MAC addresses, and switch ports is now much more complex and very dynamic (due to live migration technologies).

As the proliferation of virtualization continues, this trend toward increased complexity also continues unabated. How, then, are we supposed to address this? NIV is intended to help address this problem. NIV seeks to remove the complexity from the edge---the NICs and vNICs---and drive that complexity toward the bridges. That is a key underlying principle behind NIV. Look back at the definitions: one characteristic of an IV-capable bridge is that the IV-capable bridge and all of its associated IVs appear to the outside world _as a single bridge._

For example, consider a Nexus 2000 fabric extender connected to a Nexus 5000 switch. From both a management perspective as well as a networking perspective, the Nexus 5000+Nexus 2000 combination appears as a single device. The Nexus 2000 is simply an extension of the Nexus 5000 (hence the name "fabric extender"). This is why we say that a Nexus 2000 is an example of a interface virtualizer and why a Nexus 5000 is provided as an example of an IV-capable bridge. Similarly, this is why an IOM in the back of a UCS blade chassis functions as an interface virtualizer; it acts as an integrated part of the UCS 6100XP fabric interconnect. Just as the ports on a Nexus 2000 appear to be part of the Nexus 5000, the ports on a UCS IOM appear to be part of the UCS 6100XP fabric interconnects.

By now it should start making a little bit of sense. IVs allow you to scale to larger port densities, but they keep the complexity away from the edge of the network. If you're astute, though, you'll note that the examples I've provided so far don't address the growth of vNICs created by virtualization. The Nexus 2000 and the UCS IOM help address the growth of physical ports, but not virtual ports. Can NIV help address vNICs as well?

So far, the examples I've provided of IVs have been IVs that embrace the bridge/switch form factor. There's no reason, though, that an IV must look or feel like existing bridges or switches. Here's another type of interface virtualizer that will really blow you away: what about the Cisco Virtual Interface Controller (VIC), aka Palo? Think about it: a VIC is really just an IV built into a UCS server blade. It appears to the outside world as additional ports on the IV-capable bridge (the UCS 6100XP fabric interconnect) to which it is connected. This underscores the flexibility of IVs and also underscores the fact that IVs can be "chained," just as the VIC (an IV built onto the server blade) is "chained" behind the UCS IOM (an IV in the rear of the UCS chassis). Chained IVs appear as part of the upstream IV-capable bridge to which they are connected.

By building the IV into the server itself and leveraging hypervisor bypass (VMDirectPath in VMware vSphere), Cisco can address the growth of vNICs. With VIC as an IV on a VMware vSphere host, the ports that connect directly to a VM appear as ports on the upstream IV-capable bridge (the UCS 6100XP in this case); this erases any distinction between physical NICs on physical servers and virtual NICs on virtual servers. Again, a key component of NIV is that an IV-capable bridge and all associated IVs (regardless of form factor) appear _as a single bridge_.

Related to NIV is the idea of an Ethernet Host Virtualizer (EHV). EHV describes the behavior when an IV-capable bridge and associated IVs appear to the rest of the network as a single host. Because it appears as a single host, all issues with multiple uplinks and STP now go away. This is why, for example, the uplinks on the UCS 6100XP fabric interconnects are always active-active. I'm planning on delving a bit deeper in EHV in the near future.

I hope that this discussion of NIV has been useful. If you are interested in some additional information, I found some documents which were extremely helpful in solidifying my information. These documents ([here](http://ieee802.org/1/files/public/docs2008/new-dcb-pelissier-NIV-Simpification-0908.pdf), [here](http://ieee802.org/1/files/public/docs2008/new-dcb-pelissier-NIV-Proposal-1108.pdf), and [here](http://ieee802.org/1/files/public/docs2008/new-dcb-pelissier-NIV-Review-0109.pdf)) are very technical but they are good sources of information nevertheless.

In a future post, I'll discuss the role of the Nexus 1000V in NIV and how it relates to the other components described here.

Feel free to post any questions, comments, or clarifications below.

[1]: {{< relref "2009-11-27-understanding-npiv-and-npv.md" >}}
