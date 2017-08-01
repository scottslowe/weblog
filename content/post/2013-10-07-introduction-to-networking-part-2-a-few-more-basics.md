---
author: slowe
categories: Education
comments: true
date: 2013-10-07T09:00:00Z
slug: introduction-to-networking-part-2-a-few-more-basics
tags:
- Networking
- VLAN
- LACP
title: 'Introduction to Networking: Part 2, A Few More Basics'
url: /2013/10/07/introduction-to-networking-part-2-a-few-more-basics/
wordpress_id: 3305
---

In [part 1][1] of this series, I covered some networking basics (OSI and DoD models; layer 2 vs. layer 3; bridging, switching, and routing; Spanning Tree Protocol; and ARP and flooding). In this part, I'm going to build on those basic concepts to introduce a few more fundamental building blocks. As the series progresses, I'll continue to build on concepts and technologies introduced in earlier sections.

In this part, we'll discuss:

* VLANs

* VLAN Trunks

* Link Aggregation

I'll start with VLANs.

## VLANs

Recall that in [part 1][1] I defined a _broadcast domain_ as all the devices and hosts that are connected by bridges or switches (which operate at layer 2, the Data Link layer, of the OSI model). This means that, by default, every host plugged into a switch will automatically be part of the same broadcast domain. But what if you wanted to have multiple broadcast domains on the same switch? A _virtual LAN_, aka a _VLAN_, allows you to have this ability. Defined in its simplest terms, a VLAN is a layer 2 broadcast domain. A switch that supports VLANs supports the ability to be "subdivided" into multiple broadcast domains. In order for traffic to pass from one VLAN to another VLAN (i.e., from one broadcast domain into another broadcast domain), a layer 3 router is needed.

VLANs work by leveraging a 12-bit identifier in the Ethernet frame format (see [here](https://en.wikipedia.org/wiki/IEEE_802.1Q) for more details). This 12-bit identifier, often referred to as the VLAN tag, allows for up to 4,094 VLANs (212 = 4,096, with all zeroes [0x000 hexadecimal] and all ones [0xFFF hexadecimal] reserved). Not all switches from all vendors support using all 4,094 VLANs; some switches might support fewer than that.

&lt;aside&gt;As a side note, you might note that 802.1Q doesn't actually _encapsulate_ the original Ethernet frame. In other words, it doesn't wrap its own headers before and after the original Ethernet frame; rather, it injects the 12-bit VLAN tag into the frame just after the source MAC header.&lt;/aside&gt;

Although adding support for VLANs to a switch allows that switch to support multiple broadcast domains, it doesn't change certain layer 2 switching behaviors. In particular, the switch may still need to use flooding (described in part 1) to learn which MAC addresses are associated with which switch ports. The key difference is that flooding will only occur _within_ a VLAN, since flooding is limited to a broadcast domain.

Finally, it's important to note that VLANs themselves are strictly layer 2 constructs, but because a layer 3 router is needed to pass traffic between them, VLANs are often associated with IP subnets---meaning that each VLAN is considered a unique IP network. It's probably for this reason that some people use the terms "IP subnet" and "VLAN" interchangeably when, as you can see, they aren't necessarily the same thing. (You could, for example, have two different IP subnets running on the same broadcast domain.) Generally speaking, though, it's very common for each VLAN to represent a unique IP subnet.

## VLAN Trunks

OK, so a VLAN allows me to subdivide a switch into multiple broadcast domains. What if I have multiple switches? The IEEE 802.1Q standard that defines VLANs also defines a way for two switches to multiplex VLANs over a single link. (Without this functionality, the only way to connect multiple switches together while still preserving VLANs would be to have separate physical connections for each VLAN---clearly not very efficient.) A connection that carries multiple VLANs is often referred to as a _VLAN trunk_ (although some might say that this is a very Cisco-centric term). Note that VLAN trunks don't handle switch configuration; if you want two switches connected by a VLAN trunk to "share" the same VLANs, you'll still need to configure the VLANs on each switch.

Allow me to use a practical example to help illustrate this point. Assume you have SwitchA that has two VLANs defined (VLAN 100 and VLAN 200). Further assume that you have SwitchB with two VLANs, VLAN 200 and VLAN 300, defined. No layer 3 router is present. A VLAN trunk connects SwitchA and SwitchB. Here's the resulting connectivity matrix:

1. HostA in VLAN 100 attached to SwitchA won't be able to communicate with any hosts attached to SwitchB (there is a VLAN trunk between the switches, but SwitchB doesn't have VLAN 100 defined and VLAN 200 is a separate broadcast domain).

2. HostA in VLAN 200 attached to SwitchA will be able to communicate with hosts in VLAN 200 attached to SwitchB (the VLAN is defined on both switches and there is a VLAN trunk between the switches).

3. HostB in VLAN 200 attached to SwitchB will be able to communicate with hosts in VLAN 200 attached to SwitchA (the VLAN is defined on both switches and there is a VLAN trunk between the switches).

4. HostC in VLAN 300 attached to SwitchB will not be able to communicate with any hosts attached to SwitchA (there is a VLAN trunk between the switches, but SwitchA doesn't have VLAN 300 defined, and the VLANs that _are_ defined are separate broadcast domains).

(There are a few "gotchas" to this relatively simple example, like native/untagged VLANs and VLAN pruning across the trunk, but this discussion should suffice for now.)

The key takeaway is that in order for VLANs to span multiple physical switches, you need a) matching VLAN configurations across the physical switches, and b) a VLAN trunk connecting the physical switches. Without both of these, connectivity generally won't work. When both of these conditions are present, the VLAN (and therefore the broadcast domain) is extended across both switches. It should be fairly obvious at this point that by extending the VLAN across both switches, you've subjected ports in that VLAN on both switches to broadcasts and flooding (because they are in the same broadcast domain).

(Side note: STP also had to be modified to account for VLANs and VLAN trunks to ensure that bridging loops were not created within each VLAN. This gave rise to a group of STP versions, such as PVST and the like.)

## Link Aggregation

In [part 1][1] I introduced Spanning Tree Protocol (STP) (more information [here](https://en.wikipedia.org/wiki/Spanning_Tree_Protocol)), which the networking experts created to eliminate switching loops (which were a Bad Thing because there is no TTL-like mechanism at layer 2 to remove "old" or "stale" traffic from the network). While preventing switching loops is a Good Thing, one by-product of STP is that it blocks redundant switch-to-switch connections in the same broadcast domain. So what could you do if you needed more bandwidth between switches than a single link could offer?

This is where _link aggregation_ comes into play. Link aggregation allows switches to combine multiple physical links into one logical link, allowing for a greater amount of aggregate bandwidth between the switches. For example, I might configure 4 individual 1 Gbps physical links into a single logical link, giving me a total of 4 Gbps aggregate throughput between the two switches. The key phrase here is _aggregate throughput_; it's really important to understand that any single traffic flow will only be able to use a single physical link.

Let me explain why. Let's suppose that you have 4 links combined into a single logical link between two switches (I'll be creative and call them SwitchA and SwitchB). When traffic enters SwitchA bound for SwitchB, SwitchA needs to decide how to place the traffic on the individual members of the logical link. Switches generally support a variety of load balancing mechanisms, including source-destination MAC addresses, source-destination IP addresses, and sometimes even layer 4 (TCP/UDP) source-destination ports. For the purposes of this discussion, I'll assume that the load balancing is being done based on source and destination IP address. The traffic enters SwitchA, originating from HostA and bound for HostB attached to SwitchB. There is a link aggregate configured between SwitchA and SwitchB, so SwitchA performs a hash of the source and destination IP addresses. The result of that hash---which would be a number ranging from 0 to 3, since there are 4 links in the link aggregate---tells SwitchA which physical link to use in the logical link. SwitchA places the traffic on the physical link and off it goes. When HostB replies, SwitchB has to go through the same process, so it calculates a hash based on the source and destination IP addresses, determines which link in the aggregate to use, and off it goes.

There are a couple of key takeaways from this:

1. Both switches need to be configured to use link aggregation, and the configuration has to match on each end. If SwitchA was configured to use 2 links but SwitchB was configured to use 4 links, we'd very likely run into issues.

2. Any given traffic flow between two endpoints will _always_ be limited to the bandwidth of a single link within the aggregate. Why? Look back to the explanation above: the switches will create a hash based on some variables (MAC addresses, IP addresses, source and destination TCP/UDP ports) to determine which link to use. As long as all the variables are the same---which they would be for a single traffic flow---the hash is deterministic and always returns the same result. Therefore, the same link is always used for that particular traffic flow.

Takeaway #2 has significant implications, which I'll explore in more detail in future posts. (Here's [an example][2]; see slides 5 through 7 in particular.)

There are a number of protocols involved in link aggregation; the most common protocol is Link Aggregation Control Protocol (LACP), which is designed to enable switches to negotiate the use of link aggregation between them (when used properly, this helps address takeaway #1). You may also see references to "EtherChannel"; this is a Cisco-specific term that is also used to describe the use of link aggregation.

That's probably enough information for now. If you have any questions about any of the information I've presented here, please feel free to speak up in the comments. I welcome all courteous comments, so join in the discussion!

[1]: {{< relref "2013-08-12-introduction-to-networking-part-1-the-basics.md" >}}
[2]: {{< relref "2012-07-03-vsphere-on-nfs-design-considerations-presentation.md" >}}
