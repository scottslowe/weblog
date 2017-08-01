---
author: slowe
categories: Explanation
comments: true
date: 2014-03-21T12:30:28Z
slug: a-follow-up-on-linux-network-namespaces
tags:
- CLI
- Linux
- Networking
title: A Follow Up on Linux Network Namespaces
url: /2014/03/21/a-follow-up-on-linux-network-namespaces/
wordpress_id: 3425
---

Some time ago, I introduced you to the idea of Linux network namespaces, and provided an overview of some of the commands needed to interact with network namespaces. In this post, I'll follow up on that post with some additional information on using network namespaces with other types of network interfaces.

In the [previous network namespaces article][1], I mentioned (incorrectly, it turns out) that you had to use virtual Ethernet (veth) interfaces in order to connect a namespace to the physical network:

>It turns out you can only assign virtual Ethernet (veth) interfaces to a network namespace.

I say "incorrectly" because you _are_ able to assign more than just virtual Ethernet interfaces to a Linux network namespace. I'm not sure why I arrived at that conclusion, because subsequent testing---using Ubuntu 12.04 LTS as with the original testing---showed no problem assigning physical interfaces to a particular network namespace. The VMs I used for the subsequent testing are using a different kernel (the 3.11 kernel instead of whatever I used in the previous testing), so it's entirely possible that's the difference. If I get the opportunity, I'll try with earlier kernel builds to see if that makes any difference.

In any case, assigning other types of network interfaces to a network namespace is just like assigning veth interfaces. First, you create the network namespace:

    ip netns add <new namespace name>

Then, you'd assign the interface to the namespace:

    ip link set <device name> netns <namespace name>

For example, if you wanted to assign `eth1` to the "blue" namespace, you'd run this:

    ip link set eth1 netns blue

Please note that I haven't found a way to unassign an interface from a network namespace other than deleting the namespace entirely.

If you want to assign a VLAN interface to a namespace, the process is _slightly_ different. You'll have to create the namespace first, as with physical and veth interfaces, but you'll also have to create the VLAN interface in the "default" namespace first, then move it over to the desired namespace.

For example, first you'd create the namespace "red":

    ip netns add red

Then you'd create the VLAN interface for VLAN 100 on physical interface `eth1`:

    ip link add link eth1 name eth1.100 type vlan id 100

The generic form of this command is this:

    ip link add link <physical device> name <VLAN device name> type vlan id <VLAN ID>

Note that you can't use `ip netns exec` to run the command to create the VLAN interface in the network namespace directly; it won't work because the parent interface upon which the VLAN interface is based doesn't exist in the namespace. So you'll create the VLAN interface in the default namespace first, then move it over.

Once the VLAN interface is created, then move it to the target namespace:

    ip link set eth1.100 netns red

One (sort of) interesting thing I noted in my testing was that link status and IP addresses don't move between namespaces. Therefore, don't bother assigning an IP address or setting the link state of an interface before you move it to the final namespace, because you'll just have to do it again.

To make the interface functional inside the target namespace, you'll use `ip netns exec` to target the specific configuration commands against the desired namespace. For example, if the VLAN interface `eth1.100` exists in the namespace "blue", you'd run these commands:

    ip netns exec blue ip addr add 10.1.1.1/24 dev eth1.100

That adds the IP address to the interface in the namespace; then you'd use this command to set the link status to up:

    ip netns exec blue ip link set eth1.100 up

Then you can test network connectivity with our good friend `ping` like this:

    ip netns exec blue ping -c 4 10.1.1.2

(Obviously, you'd want to substitute an appropriate IP address there for your specific configuration and environment.)

I hope this additional information on working with Linux network namespaces is useful. As always, I invite and encourage any questions, thoughts, or corrections in the comments below. All courteous comments are welcome!

[1]: {{< relref "2013-09-04-introducing-linux-network-namespaces.md" >}}
