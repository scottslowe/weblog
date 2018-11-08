---
author: slowe
categories: Tutorial
comments: true
date: 2012-10-22T09:00:00Z
slug: wrapping-libvirt-virtual-networks-around-open-vswitch-fake-bridges
tags:
- Interoperability
- Libvirt
- Linux
- Networking
- OSS
- OVS
- Virtualization
title: Wrapping libvirt Virtual Networks Around Open vSwitch Fake Bridges
url: /2012/10/22/wrapping-libvirt-virtual-networks-around-open-vswitch-fake-bridges/
wordpress_id: 2901
---

In [a previous article][1], I talked about how to use [Open vSwitch](http://openvswitch.org) (OVS) fake bridges to bring VLAN support into your environment. In this article, I show you how to wrap a [libvirt](http://libvirt.org) virtual network around your OVS fake bridge.

You might ask, "Why wrap a libvirt virtual network around an OVS fake bridge when you can just use the OVS bridge directly?" That's a good question, and---to be perfectly honest---I don't have a great answer. At first glance, it seems like it _might_ make things easier if you are mixing both OVS-based networks and other types of networks, but I don't know that for certain. If anyone has any feedback one way or the other (why this is a good idea or why it's not a good idea), please speak up in the comments.

Now that we have that out of the way, the process for using a libvirt virtual network with an OVS fake bridge is actually pretty straightforward. First, create the appropriate OVS fake bridges using the instructions in [this article][1]. So, for example, you might create a fake bridge for VLAN 100 like this:

    ovs-vsctl add-br vlan100 ovsbr0 100

Next, create an XML definition for a libvirt virtual network. For a fake bridge named `vlan100`, your XML definition might look something like this:

```xml
<network>
  <name>vlan100-net</name>
  <forward mode='bridge'/>
  <bridge name='vlan100'/>
</network>
```

Then, in the guest domain configuration, you reference the libvirt virtual network instead of the underlying bridge directly, like this:

```xml
<interface type='network'>
  <mac address='11:22:33:aa:bb:cc'/>
  <source network='vlan100-net'/>
</interface>
```

And that's it! Based on my testing, it even appears that you can make this change on the fly, without having to reboot the guest domain. However, I could be wrong---if anyone knows definitively, please speak up in the comments. Any other corrections, clarifications, or questions are also welcome in the comments below.

[1]: {{< relref "2012-10-19-vlans-with-open-vswitch-fake-bridges.md" >}}
