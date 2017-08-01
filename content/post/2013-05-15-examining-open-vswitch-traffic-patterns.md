---
author: slowe
categories: Explanation
comments: true
date: 2013-05-15T10:52:10Z
slug: examining-open-vswitch-traffic-patterns
tags:
- Linux
- Networking
- OVS
title: Examining Open vSwitch Traffic Patterns
url: /2013/05/15/examining-open-vswitch-traffic-patterns/
wordpress_id: 3185
---

In this post, I want to provide some additional insight on how the use of [Open vSwitch (OVS)](http://openvswitch.org/) affects---or doesn't affect, in some cases---how a Linux host directs traffic through physical interfaces, OVS internal interfaces, and OVS bridges. This is something that I had a hard time understanding as I started exploring more advanced OVS configurations, and hopefully the information I share here will be helpful to others.

To help structure this discussion, I'm going to walk through a few different OVS configurations and scenarios. In these scenarios, I'll use the following assumptions:

* The physical host has four interfaces (eth0, eth1, eth2, and eth3)

* The host is running Linux with KVM, libvirt, and OVS installed

## Scenario 1: Simple OVS Configuration

In this first scenario let's look at a relatively simple OVS configuration, and examine how Linux host and guest domain traffic moves into or out of the network.

Let's assume that our OVS configuration looks something like this (this is the output from `ovs-vsctl show`):

    bc6b9e64-11d6-415f-a82b-5d8a61ed3fbd
        Bridge "br0"
            Port "br0"
                Interface "br0"
                    type: internal
            Port "eth0"
                Interface "eth0"
        Bridge "br1"
            Port "br1"
                Interface "br1"
                    type: internal
            Port "eth1"
                Interface "eth1"
        ovs_version: "1.7.1"

This is a pretty simple configuration; there are two bridges, each with a single physical interface. Let's further assume, for the purposes of this scenario, that eth2 has an IP address and is working properly to communicate with other hosts on the network. The eth3 interface is shutdown.

So, in this scenario, how does traffic move into or out of the host?

1. _Traffic from a guest domain:_ Traffic from a guest domain will travel through the OVS bridge to which it is attached (you'd see an additional "vnet0" port and interface appear on that bridge when you start the guest domain). So, a guest domain attached to br0 would communicate via eth0, and a guest domain attached to br1 would communicate via eth1. No real surprises here.

2. _Traffic from the Linux host:_ Traffic from the Linux host itself will **not** communicate over any of the configured OVS bridges, but will instead use its native TCP/IP stack and any configured interfaces. Thus, since eth2 is configured and operational, all traffic to/from the Linux host itself will travel through eth2.

The interesting point (to me, at least) about #2 above is that this _includes traffic from the OVS process itself._ In other words, if the OVS process(es) need to communicate across the network, they won't use the bridges---they'll use whatever interfaces the Linux host uses to communicate. This is one thing that threw me off: because OVS is itself a Linux process, when OVS needs to communicate across the network it will use the Linux network stack to do so. In this scenario, then, OVS would **not** communicate over any configured bridge, but instead using eth2. (This makes perfect sense now, but I recall that it didn't earlier. Maybe it's just me.)

## Scenario 2: Adding Bonding

In this second scenario, our OVS configuration changes only slightly:

    bc6b9e64-11d6-415f-a82b-5d8a61ed3fbd
        Bridge "br0"
            Port "br0"
                Interface "br0"
                    type: internal
            Port "bond0"
                Interface "eth0"
                Interface "eth1"
        ovs_version: "1.7.1"

In this case, we're now leveraging a bond that contains two physical interfaces (eth0 and eth1). (By the way, I have a write-up on [configuring OVS and bonds][1], if you need/want more information.) The eth2 interface still has an IP address assigned and is up and communicating properly. The physical eth3 interface is shutdown.

How does this affect the way in which traffic is handled? _It doesn't, really._ Traffic from guest domains will still travel across br0 (since this is the only configured OVS bridge), and traffic from the Linux host---including traffic from OVS itself---will still use whatever interfaces are determined by the host's TCP/IP stack. In this case, that would be eth2.

## Scenario 3: The Isolated Bridge

Let's look at another OVS configuration, the so-called "isolated bridge". This is a configuration that is commonly found in implementations using NVP, OpenStack, and others, and it's a configuration that I recently discussed in [my post on GRE tunnels and OVS][2].

Here's the configuration:

    bc6b9e64-11d6-415f-a82b-5d8a61ed3fbd
        Bridge "br0"
            Port "br0"
                Interface "br0"
                    type: internal
            Port "bond0"
                Interface "eth0"
                Interface "eth1"
        Bridge "br-int"
            Port "br-int"
                Interface "br-int"
                    type: internal
            Port "gre0"
                Interface "gre0"
                    type: gre
                    options: {remote_ip="192.168.1.100"}
        ovs_version: "1.7.1"

As with previous configurations, we'll assume that eth2 is up and operational, and eth3 is shutdown. So how does traffic get directed in this configuration?

1. _Traffic from guest domains attached to br0:_ This is as before---traffic will go out one of the physical interfaces in the bond, according to the bonding configuration (active-standby, LACP, etc.). Nothing unusual here.

2. _Traffic from the Linux host:_ As before, traffic from processes on the Linux host will travel out according to the host's TCP/IP stack. There are no changes from previous configurations.

3. _Traffic from guest domains attached to br-int:_ Now, this is where it gets interesting. Guest domains attached to br-int (named "br-int" because in this configuration the isolated bridge is often called the "integration bridge") don't have any physical interfaces they can use; they can only use the GRE tunnel. Here's the "gotcha", so to speak: the GRE tunnel is created and maintained by the OVS process, and therefore it uses the host's TCP/IP stack to communicate across the network. Thus, traffic from guest domains attached to br-int would hit the GRE tunnel, which would travel through eth2.

I'll give you a second to let that sink in.

Ready now? Good! The key to understanding #3 is, in my opinion, understanding that the tunnel (a GRE tunnel in this case, but the same would apply to a VXLAN or STT tunnel) is _created and maintained by the OVS process._ Thus, because it is created and maintained by a process on the Linux host (OVS itself), the traffic for the tunnel is directed according to the host's TCP/IP stack and IP routing table(s). In this configuration, the tunnels don't travel through any of the configured OVS bridges.

## Scenario 4: Leveraging an OVS Internal Interface

Let's keep ramping up the complexity. For this scenario, we'll use an OVS configuration that is the same as in the previous scenario:

    bc6b9e64-11d6-415f-a82b-5d8a61ed3fbd
        Bridge "br0"
            Port "br0"
                Interface "br0"
                    type: internal
            Port "bond0"
                Interface "eth0"
                Interface "eth1"
        Bridge "br-int"
            Port "br-int"
                Interface "br-int"
                    type: internal
            Port "gre0"
                Interface "gre0"
                    type: gre
                    options: {remote_ip="192.168.1.100"}
        ovs_version: "1.7.1"

The difference, this time, is that we'll assume that eth2 and eth3 are both shutdown. Instead, we've assigned an IP address to the br0 interface on bridge br0. OVS internal interfaces, like br0, can appear as "physical" interfaces to the Linux host, and therefore can be assigned IP addresses and used for communication. This is the approach I used in describing [how to run host management across OVS][3].

Here's how this configuration affects traffic flow:

1. _Traffic from guest domains attached to br0:_ No change here. Traffic from guest domains attached to br0 will continue to travel across the physical interfaces in the bond (eth0 and eth1, in this case).

2. _Traffic from the Linux host:_ This time, the only interface that the Linux host has is the br0 internal interface. The br0 internal interface is attached to br0, so all traffic from the Linux host will travel across the physical interfaces attached to the bond (again, eth0 and eth1).

3. _Traffic from guest domains attached to br-int:_ Because Linux host traffic is directed through br0 by virtue of using the br0 internal interface, this means that tunnel traffic is also directed through br0, as dictated by the Linux host's TCP/IP stack and IP routing table(s).

As you can see, assigning an IP address to an OVS internal interface has a real impact on the way in which the Linux host directs traffic through OVS. This has both positive and negative impacts:

* One positive impact is that it allows for Linux host traffic (such as management or tunnel traffic) to take advantage of OVS bonds, thus gaining some level of NIC redundancy.

* A negative impact is that OVS is now "in band," so upgrades to OVS will be disruptive to all traffic moving through OVS---which could potentially include host management traffic.

Let's take a look at one final scenario.

## Scenario 5: Using Multiple Bridges and Internal Interfaces

In this configuration, we'll use an OVS configuration that is very similar to the configuration I showed in my post on [GRE tunnels with OVS][2]:

    bc6b9e64-11d6-415f-a82b-5d8a61ed3fbd
        Bridge "br0"
            Port "br0"
                Interface "br0"
                    type: internal
            Port "mgmt0"
                Interface "mgmt0"
                    type: internal
            Port "bond0"
                Interface "eth0"
                Interface "eth1"
        Bridge "br1"
            Port "br1"
                Interface "br1"
                    type: internal
            Port "tep0"
                Interface "tep0"
                    type: internal
            Port "bond1"
                Interface "eth2"
                Interface "eth3"
        Bridge "br-int"
            Port "br-int"
                Interface "br-int"
                    type: internal
            Port "gre0"
                Interface "gre0"
                    type: gre
                    options: {remote_ip="192.168.1.100"}
        ovs_version: "1.7.1"

In this configuration, we have three bridges. br0 uses a bond that contains eth0 and eth1; br1 uses a bond that contains eth2 and eth3; and br-int is an isolated bridge with no physical interfaces. We have two "custom" internal interfaces, mgmt0 (on br0) and tep0 (on br1), to which IP addresses have been assigned and which are successfully communicating across the network. We'll assume that mgmt0 and tep0 are on different subnets, and that tep0 is assigned to the 192.168.1.0/24 subnet.

How does traffic flow in this scenario?

1. _Traffic from guest domains attached to br0:_ The behavior here is as it has been in previous configurations---guest domains attached to br0 will communicate across the physical interfaces in the bond.

2. _Traffic from the Linux host:_ As it has been in previous scenarios, traffic from the Linux host is driven by the host's TCP/IP stack and IP routing table(s). Because mgmt0 and tep0 are on different subnets, traffic from the Linux host will go out either br0 (for traffic moving through mgmt0) or br1 (for traffic moving through tep0), and thus will utilize the corresponding physical interfaces in the bonds on those bridges.

3. _Traffic from guest domains attached to br-int:_ Because the GRE tunnel is on the 192.168.1.0/24 subnet, traffic for the GRE tunnel---which is created and maintained by the OVS process on the Linux host itself---will travel through tep0, which is attached to br1. Thus, the physical interfaces eth2 and eth3 would be leveraged for the GRE tunnel traffic.

## Summary

The key takeaway from this post, in my mind, is understanding where traffic originates, and separating the idea of OVS as a switching mechanism (to handle guest domain traffic) as well as a Linux host process itself (to create and maintain tunnels between hosts).

Hopefully this information is helpful. I am, of course, completely open to your comments, questions, and corrections, so feel free to speak up in the comments below. Courteous comments are always welcome!

[1]: {{< relref "2012-10-19-link-aggregation-and-lacp-with-open-vswitch.md" >}}
[2]: {{< relref "2013-05-07-using-gre-tunnels-with-open-vswitch.md" >}}
[3]: {{< relref "2012-10-30-running-host-management-on-open-vswitch.md" >}}
