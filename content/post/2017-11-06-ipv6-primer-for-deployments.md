---
author: slowe
categories: Liveblog
comments: true
date: 2017-11-06T21:00:00Z
tags:
- OpenStack
- Networking
- Neutron
- OVS
- VXLAN
- IPv6
title: IPv6 Primer for Deployments
url: /2017/11/06/ipv6-primer-for-deployments/
---

This is a liveblog of the OpenStack Summit Sydney session titled "IPv6 Primer for Deployments", led by Trent Lloyd from Canonical. IPv6 is a topic with which I know I need to get more familiar, so attending this session seemed like a reasonable approach.<!--more-->

Lloyd starts with some history. IPv6 was released in 1980, and uses 32-bit address (with a total address space of around 4 billion). IPv4, as most people know, is still used for the majority of Internet traffic. IPv6 was released in 1998, and uses 128-bit addresses (for a theoretical total address space of 3.4 x 10 to the 38th power). IPv5 was an experimental protocol, which is why the IETF used IPv6 as the version number for the next production version of the IP protocol.

Lloyd shows a graph showing the depletion of IPv4 address space, to help attendees better understand the situation with IPv4 address allocation. The next graph Lloyd shows illustrates IPv6 adoption, which---according to Google---is now running around 20% or so. (Lloyd shared that he naively estimated IPv4 would be deprecated in 2010.) In Australia it's still pretty difficult to get IPv6 support, according to Lloyd.

Next, Lloyd reviews decimal and binary number conversion as a precursor to better understand how both IPv4 and IPv6 numbers are represented for human consumption, and how subnetting is handled by masking out some number of bits. Whereas IPv4 uses dotted decimal format, IPv6 uses hexadecimal numbering. Lloyd reminds attendees that MAC addresses also use hexadecimal, though only with 48 bits (instead of 128 bits with IPv6).

Bringing up an IPv6 address, Lloyd shares a few tricks to make the address easier to handle. For example, you can remove leading zeroes. You can also use a double colon (`::`) to remove consecutive series of all zeroes (this can only be done once per address). Using a colon in the address (for example, `2001:db8::8a2e:0:4`) means we have to enclose the address in square brackets before appending a colon and a port number.

Subnetting in IPv6 is handled a bit differently, naturally, given the expanded address space. Large ISPs would generally get a /32 prefix (resulting in 65,536 /48 prefixes). Large organizations would generally get a /48 prefix (resulting in 65,536 /64 prefixes, or 256 /56 prefixes). A /56 prefix is for a typical end-user site, which gives an end-user 256 /64 prefixes. And a /64 prefix is considered a single local network.

IPv6 also has some special addresses. The first special address is a link local address (similar to the "169.254" subnet in IPv4). This link local address is derived from the MAC hardware address (using EUI-64, which pads out the 48-bit MAC address to 64 bits and flips a bit to note that it was automatically generated). The link local address is always kept, even if the interface is assigned a global address. The RFC1918 equivalent of link local in IPv6 is called ULA (Unique Local Addressing) is fd00::/8 followed by a randomly-generated section (gives about a trillion unique combinations, making address space collisions far less likely).

The next topic that Lloyd addresses is Neighbour Discovery (ND), the IPv6 equivalent of ARP in IPv4. ND uses the link local address as the source, and a multicast address as the target. Address configuration---specifically, automatic address configuration---is handled via one of two methods. The first is SLAAC, which combines EUI-64 and the link prefix with a Router Advertisement (RA) to assign addresses. Early versions of SLAAC didn't support providing DNS server addresses, but this has been addressed in more recent versions of SLAAC. Users can also use DHCPv6, which operates much in the same way as DHCPv4. DHCPv6 has the option of including additional information that isn't supported when using RAs. RAs can point hosts to use DHCPv6. There's also a method of getting the address via SLAAC but configuration via DHCPv6. Finally, there's also something called DHCPv6-PD (Prefix Designation), which allows networks to request a prefix from an ISP to then handle themselves.

Lloyd talked briefly about MTU, but I missed the information. NAT isn't generally used with IPv6, although preliminary support for it is found in very recent Linux kernels. Another consideration Lloyd mentions is single stack vs. dual stack (only IPv6 or both IPv6+IPv4); most initial deployments will use dual stack.

At this point, Lloyd shifts his focus to talk specifically about IPv6 in OpenStack. Generally speaking, OpenStack has relatively strong IPv6 support throughout. The exception to this is Open vSwitch, which currently can't do GRE tunneling using IPv6 in the underlay. The fix for this is to replace GRE with VXLAN or Geneve, both of which can use IPv6 in the underlay. Lloyd also reminds attendees that they'll need to check with vendors to verify IPv6 support in their plugins or commercial solutions.

With regards to IPv6 in tenant networks, tenants should use globally unique addresses. This is done via Neutron's address scopes. Then, out of that address scope, set up a subnet pool (this allows Neutron to hand out a smaller section of the overall address scope). Then, individual tenants can just create a subnet; that subnet can be pulled from a default subnet pool.

When creating a Neutron subnet, you can specify SLAAC or DHCPv6 for address configuration.

IPv6 support is weaker when it comes to DVR (Distributed Virtual Router) or L3 HA. IPv6 does work with DVR, but the traffic isn't fully distributed; there's no IPv6 support in L3 HA.

When it comes to integrating Neutron IPv6 networks with external IPv6 networks, you can do static routing, use BGP, or you can use DHCPv6-PD (Neutron can act as a DHCPv6-PD client).

Lloyd next discusses some concerns around cloud-init and VM metadata. I didn't catch all the considerations, but it sounded like the workaround was using config-drive to address the considerations.

Reverse DNS is also a consideration around IPv6 and OpenStack. This is not generally a problem, though it may be a problem if you're sending email messages directly from instances (reverse DNS will typically be necessary in that instance).

At this point, Lloyd opens the session up to questions from the audience.
