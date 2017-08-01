---
author: slowe
categories: Explanation
comments: true
date: 2008-07-16T16:32:19Z
slug: understanding-nic-utilization-in-vmware-esx
tags:
- Cisco
- ESX
- Networking
- Virtualization
- VMware
title: Understanding NIC Utilization in VMware ESX
url: /2008/07/16/understanding-nic-utilization-in-vmware-esx/
wordpress_id: 764
---

In early December of 2006, I wrote a very popular article on [VMware ESX, NIC teaming, and VLAN trunking][1]. In that article, I laid out the configuration for using both NIC teaming and VLAN trunking. In particular, the NIC teaming configuration in that article described the use of Cisco Gigabit EtherChannel for link aggregation, in which both the physical switch and the vSwitch are configured to distribute traffic across all the links between them.

Since that time, the question has come up many times: which method is better, with EtherChannel or without? Many engineers prefer _not_ to use EtherChannel (or its standardized equivalent, static LACP/802.3ad) because of the added complexity involved. It's easier to just team the NICs at the vSwitch level and leave the physical switches alone. That is true, but what about performance? And what impact does this have on NIC utilization?

There are two ways of handling NIC teaming in VMware ESX:

1. Without any physical switch configuration

2. With physical switch configuration (EtherChannel, static LACP/802.3ad, or its equivalent)

In the NIC teaming/VLAN trunking article I referenced above, I noted that there is a corresponding vSwitch configuration that matches each of these types of NIC teaming:

1. For NIC teaming without physical switch configuration, the vSwitch must be set to either "Route based on originating virtual port ID", "Route based on source MAC hash", or "Use explicit failover order"

2. For NIC teaming with physical switch configuration---EtherChannel, static LACP/802.3ad, or its equivalent---the vSwitch must be set to "Route based on ip hash"

In order to better understand how these settings and different configurations affect NIC utilization, I set out to do some tests in the lab. Most of my tests were centered around IP-based storage from the host (i.e., using NFS or iSCSI for VMDKs), and only tested two basic configurations: using "Route based on originating virtual port ID" and no link aggregation and using "Route based on ip hash" with link aggregation. Although the tests were slanted toward IP-based storage traffic, the underlying principles should be the same for other types of traffic as well. Here's what I found.

## NIC Teaming Without Link Aggregation

First, it's important to understand the basic behavior in this configuration. Because the vSwitch is set to "Route based on originating virtual port ID", network traffic will be placed onto a specific uplink and won't use any other uplinks until that uplink fails. (This is described in more detail in [this PDF](http://www.vmware.com/files/pdf/virtual_networking_concepts.pdf) from VMware.) Every VM and every VMkernel port gets its own virtual port ID. These virtual port IDs are visible using esxtop (launch esxtop, then press "n" to switch to network statistics). That's simple enough, but what does this mean in practical terms?

* Each VM will only use a single network uplink, regardless of how many different connections that particular VM may be handling. All traffic to and from that VM will be place on that single uplink, regardless of how many uplinks are configured on the vSwitch.

* Each VMkernel NIC will only use a single network uplink. This is true both for VMotion as well as IP-based storage traffic, and is true regardless of how many uplinks are configured on the vSwitch.

* Even when the traffic patterns are such that using multiple uplinks would be helpful---for example, when a VM is copying data to or from two different network locations at the same time, or when a VMkernel NIC is accessing two different iSCSI targets---only a single uplink will be utilized.

This last bullet is particularly important. Consider the implications in a VMware Infrastructure 3 (VI3) environment using the software iSCSI initiator with multiple iSCSI targets. Even though multiple iSCSI targets may be configured, **all** the iSCSI targets will share **one** uplink from that vSwitch using this configuration. Obviously, that is not ideal.

Note that this doesn't really impact VMotion traffic, since VMotion is a point-to-point type of connection. VMotion would only be impacted if placed on a vSwitch with other types of traffic and their virtual port IDs were assigned to the same uplink.

## NIC Teaming With Link Aggregation

In this configuration, EtherChannel/static LACP/802.3ad is configured on the physical switch and the ESX vSwitch is configured for "Route based on ip hash." With this configuration, the behavior changes quite dramatically.

* Traffic to or from a VM could be placed onto any uplink on the vSwitch, depending upon the source and destination IP addresses. Each pair of source and destination IP addresses could be placed on different uplinks, but any given pair of IP addresses can use only a single uplink. In other words, multiple connections to or from the VM will benefit, but each individual connection can only utilize a single link.

* Each VMkernel NIC will utilize multiple uplinks only if multiple destination IP addresses are involved. Conceivably, you could also use multiple VMkernel NICs with multiple source IP addresses, but I haven't tested that configuration.

* Traffic that is primarily point-to-point won't see any major benefit from this configuration. A single VM being accessed by another single client won't see a traffic boost other than that possibly gained by the placement of other traffic onto other uplinks.

This configuration can help improve uplink utilization on a vSwitch because traffic is dynamically placed onto the uplinks based on the source and destination IP addresses. This helps improve overall NIC utilization when there are multiple VMs or when a VMkernel NIC is accessing multiple IP-based storage targets. Note again, though, that individual connections will only ever be able to utilize a single uplink.

## Practical Application

I come back again to the question I asked earlier: what does this mean in practical terms?

* If you want to scale IP-based storage traffic, you'll have to use link aggregation **and** multiple targets. Using link aggregation with a single target (destination IP address) won't use more than a single uplink; similarly, no link aggregation with multiple targets will still result in only a single uplink being used. Only with link aggregation **and** multiple targets will multiple uplinks get used.

* Link aggregation will help with better overall uplink utilization for vSwitches hosting VMs. Because there are multiple source/destination address pairs in play, the vSwitch will spread them around the uplinks dynamically.

* To achieve the best possible uplink utilization as well as provide redundancy, you'll need physical switches that support cross-stack link aggregation. I believe the Cisco Catalyst 3750 switches do, as do the Catalyst 3120 switches for the HP c-Class blade chassis. I don't know about other vendors, since I deal primarily with Cisco.

Clearly, this has some implications for efficient and scalable VI3 designs. I'd love to hear everyone's feedback on this matter. In my humble opinion, extended conversations about this topic can only serve to better educate the community as a whole.

**UPDATE:** Reader Tim Washburn pointed out that the "Route based on Source MAC hash" actually can't be used in conjunction with link aggregation; it's behavior is identical to "Route based on originating virtual port ID". Thanks for the correction, Tim!

[1]: {{< relref "2006-12-04-esx-server-nic-teaming-and-vlan-trunking.md" >}}
