---
author: slowe
categories: Liveblog
comments: true
date: 2015-04-29T13:33:00Z
tags:
- Networking
- Interop2015
- Security
title: 'Interop Liveblog: IPv6 Microsegmentation'
url: /2015/04/29/ipv6-microsegmentation/
---

This session was titled "IPv6 Microsegmentation," and the speaker was Ivan Pepelnjak. Ivan is, of course, a well-known figure in the networking space, and publishes content at [http://ipspace.net][link-1].

The session starts with a discussion of the problems found in Layer 2 IPv6 networks. Some of the problems include spoofing RA (Router Advertisement) messages, NA (Neighbor Advertisement) messages, DHCPv6 spoofing, DAD (Duplicate Address Detection) DoS attacks, and ND (Neighbor Discovery) DoS attacks. All of these messages derive from the assumption that one subnet = one security zone, and therefore intra-subnet communications are not secured.

Note that some of these attacks are also common to IPv4 and are not necessarily unique to IPv6. The difference is that these problems are well understood in IPv4 and therefore many vendors have implemented solutions to mitigate the risks.

According to Ivan, the root cause of all these problems originates with the fact that all LAN infrastructure today emulates 40 year old thick coax cable.

The traditional fix is to add kludges....er, new features---like RA guard (prevents non-routers from sending RA messages), DHCPv6 guard (same sort of functionality), IPv6 ND inspection (same idea), and SAVI (Source Address Verification Inspection; complex idea where all these various messages are inspected and attempt to verify address ownership). The challenge with some of these fixes is that IPv6's extension headers---which come before TCP or UDP header---can sometimes be manipulated to bypass such protections. Further, these features are only vendor- and/or platform-specific, and therefore aren't broadly available to all customers and all implementations. These protections are also expensive to implement in hardware.

The proposed solution suggested by Ivan is to move to Layer 3-only IPv6 network designs. In such a design, the first-hop network device is a layer 3 switch (router). This means there is no RA spoofing, ND spoofing, or NA spoofing. But is this actually possible? Ivan indicates that some networks are already built this way---an example is T-Mobile's mobile network, which in some cases uses only IPv6. In such cases, every mobile device has its own /64 subnet. This is also true on many xDSL implementations (when PPPoX is in use). Unfortunately, it's much harder to implement on Carrier Ethernet, and almost impossible on cable networks.

For Carrier Ethernet, Cisco ME3600---a very expensive switch---will allow you to implement such an architecture. It can also be done using a dedicated VLAN for each customer, which would require extensive service automation (and may create some scale issues?).

Can we use shared Layer 3 IPv6 subnets? If the router sends RA messages without any prefixes, then devices are forced to send their packets to the router---which can then enforce any necessary security issues. Unfortunately, it doesn't address some of the spoofing attacks, so RA and ND attacks are still possible. Private VLANs (PVLANs) can address that problem, but might break DAD process (a DAD proxy on the router would be needed).

(The ND cache in IPv6 is like the ARP cache in IPv4.)

What about data centers, where workload mobility is a key requirement? VLANs are _not_ the solution, according to Ivan (I don't necessarily disagree). Some potential solutions include a VLAN per VM (not recommended) or PVLANs (complex); both of these solutions prevent aggregation due to workload mobility. These solutions may also cause problems with various vendor-specific IPv6 limitations (example given is only 3K IPv6 routes supported versus 12K IPv4 routes).

Ivan next theorizes about a potential solution in which routers detect adjacent hosts (via DAD or other messages). The first-hop router creates a host route, which are then propagated throughout the local routing domain. IPv6 ND entries are used instead of IPv6 routing table entries. This bypasses some IPv6 limitations because host routes are treated in hardware like ND cache entries (which means you get something like 32K entries).

This sort of approach is used by Hyper-V, which uses L3-only switching for both intra-hypervisor and inter-hypervisor traffic. Some orchestration system pushes entries into the hosts so that all hosts know the addresses of all other hosts. The host then proxies ARP/ND queries, and performs only L3 switching for all traffic.

Cisco's Dynamic Fabric Automation (DFA) also does only L3 switching. Via discovery of host routers via ARP/ND/DHCPv6 messages, all switches know all host routes, all fabric routes, and all external routes. Leaf switches share the same IP address and same MAC address on the "downstream" interface, so L3 forwarding is always optimal (even in workload mobility situations).

So why bother with all these solutions? First, they remove all IPv6 first-hop security challenges (the solution lies in creating point-to-point links between the host and the first-hop network device). Products that support IPv6 and IPv4 host route-based forwarding include Hyper-V Network Virtualization, Juniper Contrail, and Cisco DFA. IPv4-only implementations include Nuage Virtual Services Platform (VSP) and Cisco Application Centric Infrastructure (ACI). VMware NSX has a stateful IPv6 firewall that provides RA guard and ND inspection services. (Microsoft also has a stateful IPv6 firewall.)

With that, Ivan wraps up the session.


[link-1]: http://ipspace.net
