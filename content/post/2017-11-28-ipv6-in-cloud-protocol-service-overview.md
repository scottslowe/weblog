---
author: slowe
categories: Liveblog
comments: true
date: 2017-11-28T12:00:00Z
tags:
- AWS
- Networking
- reInvent2017
title: "Liveblog: IPv6 in the Cloud - Protocol and Service Overview"
url: /2017/11/28/ipv6-in-cloud-protocol-service-overview/
---

This is a liveblog of an AWS re:Invent 2017 breakout session titled "IPv6 in the Cloud: Protocol and Service Overview." The presenter's name is Alan Halachmi, who is a Senior Manager of Solutions Architecture at AWS. As with so many of the other breakout sessions and workshops here at re:Invent this year, the queues to get into the session are long and it's expected that the session will be completely full.<!--more-->

Halachmi starts the session promptly at 11:30am (the scheduled start time) by reviewing the current state of IP4 exhaustion, then quickly moves to a "state of the state" regarding IPv6 adoption on the Internet. Global IPv6 adoption is currently around 22%, and is expected to hit 25% by the end of the year. Mobile and Internet of Things (IoT) are driving most of the growth, according to Halachmi. T-Mobile, for example, now has 89% of their infrastructure running on IPv6.

Transitioning again rather quickly, Halachmi moves into an overview of the IPv6 protocol itself. IPv4 uses a 32-bit address space; IPv6 uses a 128-bit address space (29 orders of magnitude larger than IPv4). IPv4 uses dotted decimal with CIDR (Classless Interdomain Routing) notation; IPv6 uses colon-separated hextet notation with CIDR. To help keep IPv6 addresses more human-readable, we can omit leading zeroes. You can also use the double-colon notation to represent any contiguous group of zeroes.

IPv6 also doesn't use broadcast addresses, instead using multicast (focusing on the ff00::/8 range). IPv6 also has some special unicast addresses:

* The unspecified address (all zeroes) indicates the absence of an address; this is primarily used in Duplicate Address Detection (DAD)
* The loopback address is used to communicate with itself (the IPv6 address is ::1, equivalent to IPv4 127.0.0.1)
* The link local address (LLA), which is scoped to a specific link (they're only usable on the local link; equivalent to IPv4 169.254.0.0/16 range). In IPv4, the link local address is typically ephemeral; in IPv6, it's required. The typical LLA prefix is Fe80::/64, and the Interface Identifier (IID) is generated either a) manually; b) systematically using modified EUI-64; or c) derived randomly. DAD is used in almost all cases.
* The unique local address (ULA) can be roughly compared to RFC 1918 in the IPv4 world. However, the ULA is designed to be globally unique, and comes out of the `fc00::/7` range. Halachmi is not a fan of the ULA, and doesn't go into any additional detail.
* The Global Unicast Address (GUA), is a globally-unique address that allows IPv6-equipped workloads to communicate end-to-end without any network address translation (NAT). GUAs have both a prefix (64-bit) and an IID (also 64 bits; EUI-64 has fallen out of favor for generating the IID portion of the GUA). There are several ways to assign GUAs: manually, systematically (via Router Advertisements [RAs] and SLAAC [Stateless Address Autoconfiguration]), dynamically via DHCPv6 (materially different than DHCPv4), or randomly. DAD is almost always used to prevent duplicate addresses.

EUI-64 takes a MAC (Media Access Control) address, which is 48-bits long, and converts it to a 64-bit address. First, invert the 7th most significant bit; then insert `ff:fe` into the middle of the address (after the first 24 bits and before the second 24 bits).

So how do IPv6 nodes communicate across the network? Halachmi quickly reviews IPv4 and ARP, then moves into the equivalent in IPv6. IPv6 doesn't use ARP; it uses solicited-mode multicast, using ff02::1:ff00:0/104 and adding the least significant 24 bits from the target IPv6 node address. At layer 2, this translates into an Ethernet address in the 33:33:00:00:00:00 range (again using some of the least significant bits from the target node's IPv6 address).

Halachmi now moves on to discussing how to access resources on an IPv6 network. The first example is using either dual-stack configurations (both IPv4 and IPv6 addresses on a node) and relying on DNS A (IPv4) or DNS AAAA (IPv6) address resolution.

A few other notable differences about IPv6:

* End-to-end philosophy (no NAT, privacy is not equal to security, public IP is not equal to reachability)
* Intermediate nodes may not fragment packets (path MTU discovery using ICMPv6)
* Maximum payload per packet jumps to about 4GB
* Options extensibility with extension header chaining

Having now covered the IPv6 protocol, Halachmi moves on to discussing IPv6 at AWS. The first IPv6-enabled service at AWS was the IoT service. The IoT data plane is IPv4 and IPv6; the control plane is IPv4 only. S3 is also IPv6- and IPv4-enabled; just add `.dualstack` in the fully-qualified domain name (same goes for S3 Transfer Acceleration). CloudFront supports IPv6; just enable it in the distribution. (Halachmi does point out a couple considerations regarding the use of cookies to limit access, and points attendees to the documentation for CloudFront.)

Continuing, Halachmi points out that CloudFront will support IPv6 from clients, but talks IPv4 to the origin. This can help with transitions from IPv4 to IPv6.

WAF also supports IPv6 (has supported it since launch). Route 53 supports quad-A records (for returning an IPv6 address), and for the last year or so has also supported DNS queries over IPv6.

Coming to VPCs, Halachmi points out that VPC does support IPv6, and takes some time to drill down into a few details/tenets of the implementation. These tenets include end-to-end connectivity to deliver on the IPv6 promise, offering a dual-stack solution, and a couple others I couldn't capture. IPv6 in VPCs doesn't support ULAs. GUAs come from a fixed /56 block, and subnets are of a fixed /64 size. IPv6 in VPCs don't support privacy/temporary addresses.

What about assignment of addresses? Halachmi points out that you _can_ do manual/static assignment, but it isn't recommended. SLAAC was investigated, but this didn't work well for the AWS VPC use case. Instead, AWS uses DHCPv6 (the actual implementation is stateless and derived from the topology database).

Regarding security considerations, IPv6 is not on by default and must be enabled per interface. Even if the OS in an instance is IPv6-enabled and ends up with a LLA, the VPC won't pass IPv6 traffic until you assign a GUA (i.e., link-local connectivity doesn't work without a GUA). Route tables are not auto-updated except the GUA CIDR. AWS won't modify security groups or NACLs to allow IPv6 traffic unless the security group/NACL is unmodified/left at default settings.

To provide a semantically-similar NAT mechanism for IPv6, AWS introduced the EIGW (Egress-only Internet Gateway).

Direct Connect also supports IPv6, as do Virtual Private Gateways. Direct Connect Gateway supports IPv6 as well.

Amazon WorkSpaces supports IPv6-based communications, as does AppStream 2.0. The AWS Application Load Balancer (ALB) supports IPv6, but this must be enabled at the time of creation. ALB can also be helpful in IPv4-to-IPv6 transitions, like CloudFront.

Halachmi points out that there is still lots of work to do, and welcomes feedback from customers on how AWS should prioritize the implementation of IPv6 across their APIs and services.

At this point, Halachmi ends the session and opens up for questions.
