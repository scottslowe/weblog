---
author: slowe
categories: Tutorial
comments: true
date: 2007-06-13T14:31:30Z
slug: cisco-link-aggregation-and-netapp-vifs
tags:
- Cisco
- Interoperability
- NetApp
- Networking
- ONTAP
title: Cisco Link Aggregation and NetApp VIFs
url: /2007/06/13/cisco-link-aggregation-and-netapp-vifs/
wordpress_id: 471
---

[Network Appliance](http://www.netapp.com/) storage systems support the use of virtual interfaces (VIFs) to provide link redundancy and improved network throughput. Two types of VIFs are available:

* _Single-mode VIFs_ act like a fault tolerant team and will fail traffic over to a standby link when the active link goes down.

* _Multi-mode VIFs_ act like a group of links providing aggregate bandwidth as well as link redundancy.

Single-mode VIFs are great for fault tolerance, but the storage system isn't leveraging all the links. It's "active-passive" arrangement in which only one of the links is passing traffic while the other link is idle. No switch support is required for this configuration.

Multi-mode VIFs, on the other hand, allow for both greater bandwidth utilization as well as fault tolerance. Traffic will be distributed across all the links in the VIF (typically based on IP address), and if one link fails the traffic is redistributed across the remaining links. However, this configuration requires support on the switch. In this article, we're going to look at configuring a [Cisco Catalyst 3560](http://www.cisco.com/en/US/products/hw/switches/ps5528/index.html) switch to do link aggregation with a NetApp storage system running Data ONTAP 7.1.1.1.

To configure the switch, you would use the following commands (these are entered in global configuration mode on the switch):

```text
s3(config)#int port-channel1
s3(config-if)#description Multi-mode VIF for netapp1
s3(config-if)#int gi0/23
s3(config-if)#channel-group 1 mode on
s3(config-if)#int gi0/24
s3(config-if)#channel-group 1 mode on
```

This creates the port-channel1 interface (you may need to increment that number, i.e., use port-channel2 or port-channel3, if you already have existing link aggregates configured) and adds interfaces GigabitEthernet0/23 and GigabitEthernet0/24 to the link aggregate. If you do have to use a different link aggregate interface, be sure the number of the interface (`int port-channel 4`, for example) matches the number of the channel-group specified on the member interfaces (`channel-group 4 mode on`, for example). This seems obvious, but it's worth mentioning nevertheless.

Be aware that Data ONTAP's multi-mode VIFs are only compatible with static 802.3ad link aggregation; you can't use PAgP (Cisco proprietary protocol). I would assume dynamic LACP is also incompatible. For this reason we used the `channel-group 1 mode on` statement instead of something like `channel-group 1 mode desirable`.

By default, many Cisco switches default to MAC address-based load balancing across the links, whereas NetApp defaults to IP address-based load balancing. To see the switch's current load balancing configuration, use this command in privileged mode:

```text
s3#show etherchannel load-balance
```

To change the switch's load balancing algorith to a mode compatible with NetApp's, use one of the following command in global configuration mode (note that changing it affects the entire switch; you can't change it for a single port-channel individually):

```text
s3(config)#port-channel load-balance src-dst-ip
```

Once the switch is configured, then we can proceed with configuring the NetApp storage system. The following commands will create the the multi-mode VIF (this can also be done via the FilerView GUI):

```text
netapp1>vif create multi vif0 e6d e7d
netapp1>ifconfig vif0 172.31.254.10 netmask 255.255.255.0
netapp1>ifconfig vif0 up
```

This creates the VIF with interfaces e6d and e7d as members, plumbs it with an IP address, and brings it up. Running the command `vif status vif0` now will return the following results:

```text
default: transmit 'IP Load balancing', VIF Type 'multi_mode', fail 'log'
vif0: 2 links, transmit 'IP Load balancing', VIF Type 'multi-mode' fail 'default'

  VIF Status     Up      Addr_set
        up:
        e6d: state up, since 05Oct2001 17:17:15 (05:23:05)
                mediatype: auto-1000t-fd-up
                flags: enabled
                input packets 2000, input bytes 12800
                output packets 173, output bytes 1345
                up indications 1, broken indications 0
                drops (if) 0, drops (link) 0
                indication: up at boot
                        consecutive 3, transitions 1
        e7d: state up, since 05Oct2001 17:18:03 (00:10:03)
                mediatype: auto-1000t-fd-up
                flags: enabled
                input packets 134, input bytes 987
                output packets 20, output bytes 156
                up indications 1, broken indications 0
                drops (if) 0, drops (link) 0
                indication: broken
```

Note the 'IP Load balancing' algorithm stated in the output; this is why the switch's load-balancing mechanism should be changed to match.

At this point, the links should be up between the Cisco switch and the NetApp storage system, and traffic should be passing to and from the storage system without any problems. To test the fault tolerance, we can pull one of the links in VIF; traffic should continue to flow with very little, if any, interruption. And while traffic from a single client to the NetApp won't see a significant increase in throughput, the overall throughput of multiple separate clients to the NetApp should improve with multiple links in the VIF.

More information, including additional Cisco configs, is available [here](http://episteme.arstechnica.com/eve/forums/a/tpc/f/469092836/m/781008784831).
