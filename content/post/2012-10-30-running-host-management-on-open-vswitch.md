---
author: slowe
categories: Tutorial
comments: true
date: 2012-10-30T09:00:00Z
slug: running-host-management-on-open-vswitch
tags:
- Linux
- Networking
- OSS
- OVS
title: Running Host Management on Open vSwitch
url: /2012/10/30/running-host-management-on-open-vswitch/
wordpress_id: 2915
---

[Open vSwitch (OVS)](http://openvswitch.org) brings lots of functionality to Linux virtualization hosts, including the ability to use Link Aggregation Control Protocol (LACP) for enhanced network interface card (NIC) failover and redundancy. In this post, I'm going to show you how to run a host management interface on a LACP bundle through OVS. This prevents you from having to give up a physical interface for host management, and allows you to run all traffic through OVS.

First, configure the LACP bundle as I describe in my post on [link aggregation and LACP with OVS][1]. You'll want to ensure that the LACP configuration is working properly before you continue. Remember that you can use `ovs-appctl bond/show <bond name>` and `ovs-appctl lacp/show <bond name>` to help troubleshoot the LACP configuration and operation.

Next, create an "internal" interface that Linux will use as a Layer 3 interface. We'll do this using `ovs-vsctl`, like this:

    ovs-vsctl add-port ovsbr1 mgmt0 -- set interface mgmt0 type=internal

This command adds a port called `mgmt0` to the specified bridge (`ovsbr1` in this example), and then sets the interface associated with `mgmt0` to the type "internal." I don't yet know why setting the type to internal is required, only that it's necessary if you want the interface to be usable by Linux as a Layer 3 interface.

If you need the management traffic on a specific VLAN, then modify the command to specify a particular OVS fake bridge (more here on using [fake bridges with VLANs][2]):

    ovs-vsctl add-port vlan100br mgmt0 -- set interface mgmt0 type=internal

You can also just assign the VLAN tag directly, although that configuration goes against the OVS port, not the OVS interface:

    ovs-vsctl set port mgmt0 tag=100

Once the interface is created, then configure Linux to use that interface as a Layer 3 interface with an IP address assigned. On Ubuntu 12.04 LTS, that means adding a stanza to the `/etc/network/interfaces` configuration file, like this:

    auto mgmt0
    iface mgmt0 inet static
    address 192.168.254.100
    netmask 255.255.255.0
    gateway 192.168.254.254
    dns-nameservers 192.168.254.101

And that's it! You can now use the IP address assigned to the `mgmt0` interface (or whatever you named it) to manage the Linux virtualization host, all while knowing that you have a bonded LACP configuration protecting you against a network link failure. I'm using this configuration on all my Ubuntu-KVM hosts in my home lab; the physical interfaces are members of LACP bonds.

(Oh, one side note: on Ubuntu, this configuration causes a delay during the boot process. I don't yet know why this delay occurs, or how to fix it. I'll post something if I figure it out.)

If you have any questions or thoughts about this configuration, please feel free to speak up in the comments below---all courteous comments are welcome.

[1]: {{< relref "2012-10-19-link-aggregation-and-lacp-with-open-vswitch.md" >}}
[2]: {{< relref "2012-10-19-vlans-with-open-vswitch-fake-bridges.md" >}}
