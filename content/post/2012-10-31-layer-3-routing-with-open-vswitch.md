---
author: slowe
categories: Tutorial
comments: true
date: 2012-10-31T09:00:00Z
slug: layer-3-routing-with-open-vswitch
tags:
- Linux
- Networking
- OSS
- OVS
title: Layer 3 Routing with Open vSwitch
url: /2012/10/31/layer-3-routing-with-open-vswitch/
wordpress_id: 2918
---

[Open vSwitch (OVS)](http://openvswitch.org) is a core component in a number of prominent virtualization- and cloud-related products and projects (consider that OpenStack Quantum, CloudStack, XenServer, and Nicira NVP all leverage OVS). I've discussed before how to do [VLANs with OVS][1] ([here][2] too) as well as how to use [link aggregation with OVS][3]. In this post, I'm going to explore the use of OVS for inter-VLAN routing at Layer 3.

&lt;aside&gt;It's actually a bit of a misnomer, in my opinion, to talk about using OVS for layer 3 routing. In reality, OVS relies on the routing functionality that's built into the Linux kernel, as you'll see, and doesn't perform the routing functionality itself (as far as I have been able to determine).&lt;/aside&gt;

To configure OVS to do inter-VLAN routing, you'll need to perform the following steps:

1. Configure OVS for each VLAN.

2. Configure VLAN interfaces.

3. Enable IP routing in the Linux kernel.

Let's take a look at each of these steps in a bit more detail.

## Configuring OVS for VLANs

This is something I've written about already, so I'll refer you to one of the following articles for more information:

[Some Insight into Open vSwitch Configuration][4]  

[VLANs with Open vSwitch Fake Bridges][1]  

[Wrapping libvirt Virtual Networks Around Open vSwitch Fake Bridges][2]

Once this step is complete, you're ready to configure the VLAN interfaces.

## Configuring VLAN Interfaces

You can think of these VLAN interfaces as the equivalent of a Switched Virtual Interface (SVI) or Bridged Virtual Interface (BVI) on a multi-layer physical switch. For example, if you're familiar with Cisco switches, think of these as the `interface vlan` equivalent. In the Brocade FastIron line of switches, these would be the `interface ve` equivalent.

To create the VLAN interfaces in OVS, you would use this command:

    ovs-vsctl add-port <bridge name> <port name> -- set interface <port name> type=internal

Note that---as far as I can tell---the VLAN interfaces need to be attached to the appropriate OVS fake bridge for that particular VLAN (more information [here][1]).

I haven't (yet) found any detailed discussion of the theory (or principles) behind this command, but I can tell you that what it does is enable a "pseudo"-device that can be configured as a Layer 3 interface within Linux. (In other words, you can assign an IP address to it.) This is the same technique that I described in my post on running [host management across OVS][5].

You'll need to repeat this process for each VLAN interface you need, i.e., for each VLAN that should be routable through this OVS-enabled host.

Once the VLAN interfaces are created, then you need to configure them within Linux. The exact procedure for this varies between Linux distributions; in Ubuntu (which I generally use), this means creating the appropriate configuration stanzas within `/etc/network/interfaces`.

Let's say you wanted to route between two VLANs, VLAN 100 and VLAN 200. If the VLAN interfaces on OVS were named `vlan100` and `vlan200`, then you might configure `/etc/network/interfaces` to include something like this:

    auto vlan100
    iface vlan100 inet static
    address 192.168.100.1
    netmask 255.255.255.0
    
    auto vlan200
    iface vlan200 inet static
    address 192.168.200.1
    netmask 255.255.255.0

These are just examples, of course; you'd need to supply the appropriate IP addresses for each VLAN involved.

Once the VLAN interfaces are created and configured, you're ready for the final step---enabling IP routing (or IP forwarding) in the Linux kernel.

## Enabling IP Forwarding in the Linux Kernel

This is extremely well-documented on numerous sites around the World Wide Web; here's just [one example](http://www.ducea.com/2006/08/01/how-to-enable-ip-forwarding-in-linux/). For those who don't want to bother visiting another site, here's the quick run-down:

* To temporarily enable IP forwarding, use `sysctl -w net.ipv4.ip_forward=1`. This will **not** persist across reboots.

* To permanently enable IP forwarding, edit `sysctl.conf` so that `net.ipv4.ip_forward` is set to 1.

Once IP forwarding is enabled, you should be able to route IP traffic from one VLAN through the OVS VLAN interfaces to another VLAN. You'll need to either set the default gateway of the device(s) to be the IP address on the OVS VLAN interface, or you'll need to manually manipulate the routing table. The procedure for either of these techniques will vary according to your OS. I tested it using a Windows Server guest domain, running on one VLAN and configured to use the OVS VLAN interface as the default gateway, communicating via `ping` to another device on another VLAN. The biggest challenge was making sure the routing tables were correct, so that each device was pointing to the appropriate default gateway (which should correspond to the IP address assigned to the matching OVS VLAN interface).

Feel free to post any questions, corrections, or clarifications in the comments below. Your feedback and any courteous comments are always welcome.

[1]: {{< relref "2012-10-19-vlans-with-open-vswitch-fake-bridges.md" >}}
[2]: {{< relref "2012-10-22-wrapping-libvirt-virtual-networks-around-open-vswitch-fake-bridges.md" >}}
[3]: {{< relref "2012-10-19-link-aggregation-and-lacp-with-open-vswitch.md" >}}
[4]: {{< relref "2012-10-04-some-insight-into-open-vswitch-configuration.md" >}}
[5]: {{< relref "2012-10-30-running-host-management-on-open-vswitch.md" >}}
