---
author: slowe
categories: Musing
comments: true
date: 2012-03-12T19:49:19Z
slug: some-thoughts-and-questions-about-stt
tags:
- Networking
- Virtualization
- VXLAN
title: Some Thoughts and Questions About STT
url: /2012/03/12/some-thoughts-and-questions-about-stt/
wordpress_id: 2548
---

I just finished reading the Internet Draft for Stateless Transport Tunneling (STT), a proposed protocol for network virtualization. STT's contemporaries are VXLAN (Virtual eXtensible Local Area Network) and NVGRE (Network Virtualization using Generic Routing Encapsulation), both of which are also described in IETF Internet Drafts. The goal of all of these protocols is to virtualize (abstract) the physical network topology and bring functionality like isolation of multiple tenants, isolation of overlapping address space between multiple tenants, expanded VLAN/tenant ID address space, and enhanced VM mobility (by providing L2 services over an L3 network, for example).

I've written a little bit about VXLAN before; see my post titled [Examining VXLAN][1] for more details.

After reading the STT draft, I'm struck by a few initial thoughts:

* The STT draft proposes the use of "TCP-like" headers to take advantage of hardware-based TCP Segmentation Offload (TSO) support. The headers look enough like TCP to allow existing TSO support in network interface cards (NICs) and other devices to work, but are different enough that a STT-unaware receiver would simply drop the packets. STT even uses the same IP protocol number (6) as TCP.

* Although the STT headers "look" like TCP, there is no TCP handshake or connection state maintained. The draft authors acknowledge that this is both good and bad; it's good in that the overhead of managing TCP connections is avoided, but bad that intermediary devices (like firewalls) that might ordinarily leverage TCP state information will no longer be able to do so with STT flows.

* Like VXLAN and NVGRE, STT can be negatively impacted by "middle boxes," devices in the physical network between the STT endpoints that might potentially interfere with the traffic. Firewalls are one key example; without STT support in the firewalls, it's an either/or solution: either all STT traffic passes (based on the STT "TCP-like" destination port) or no STT passes. As I said, though, this is not unique to STT; VXLAN and NVGRE also suffer from the same issue.

* The STT protocol draft makes special accommodations to enable more efficient processing of STT frames, such as providing an "L4 offset" that allows an STT-aware device to immediately know where the start of the inner TCP/UDP payload starts (instead of having to perform extensive parsing of the headers).

* To provide an expanded address space, STT leverages a 64-bit "Context ID," which could be used in any number of ways: expanded VLAN IDs, customer/tenant IDs, etc. VXLAN, on the other hand, supplies a 24-bit ID that is explicitly called out as the VXLAN Network Identifier (VNI). Similarly, NVGRE uses a 24-bit Tenant Network Identifier (TNI). STT's Context ID is both larger and more generic.

* Like VXLAN and NVGRE, physical networking equipment will not be aware of STT.

A few questions also arise:

* Is the performance benefit realized by---as the draft authors state---having STT "explicitly designed to leverage the TSO capabilities of currently available NICs" really that significant? The STT draft states that the protocol was designed this way to improve the performance of data transfers, implying that STT's architecture and use of TSO enables it to provide better performance than VXLAN (which encapsulates inside UDP) or NVGRE (which encapsulates inside GRE).

* What are the drawbacks of re-using the TCP protocol number and repurposing TCP header structures for different purposes? Does this set a precedent that could potentially have negative repercussions? Does this open the door for vendors to "co-opt" standard protocol numbers and header structures for their own purposes, and what effect will that have on the network industry?

The one key takeaway that I have from reading the draft is just how much the "network virtualization" space is like a Wild West shoot-out: it's a big free-for-all with competing (proposed) standards and competing (proposed) protocols like VXLAN, NVGRE, and STT; each of these proposals offers some unique strengths but also has some drawbacks and limitations. Maybe I'm just naive, but it seems to me the sort of broad interoperability to which we've become accustomed in IP-based networks is in danger of being fragmented along vendor lines: VMware-based networks deploying VXLAN, Microsoft-based networks deploying NVGRE, and Nicira throwing STT in there for good measure (presumably with STT support in Open vSwitch). It will be interesting---perhaps even challenging---as these various proposals sort themselves out. That is, of course, assuming that they sort themselves out.

Courteous comments are always welcome; please feel free to speak up in the comments! Also, please disclose affiliations where appropriate; for example, although I work for EMC, these thoughts are mine and mine alone.

[1]: {{< relref "2011-12-02-examining-vxlan.md" >}}
