---
author: slowe
categories: Information
comments: true
date: 2012-11-12T11:29:10Z
slug: libvirt-ovs-integration-revisited
tags:
- Interoperability
- Libvirt
- Linux
- Networking
- OSS
- OVS
- Virtualization
title: Libvirt-OVS Integration Revisited
url: /2012/11/12/libvirt-ovs-integration-revisited/
wordpress_id: 2943
---

A few days ago, I posted an article about [using VLANs with Open vSwitch (OVS) and libvirt][1]. In that article, I stated that libvirt 1.0.0 was needed. Unfortunately (or maybe fortunately?), I was wrong. The libvirt-OVS integration works in earlier versions, too.

In my [earlier OVS-libvirt post][1], I supplied this snippet of XML to define a libvirt network that you could use with OVS:

``` xml
<network>
  <name>ovs-network</name>
  <forward mode='bridge'/>
  <bridge name='ovsbr0'/>
  <virtualport type='openvswitch'/>
  <portgroup name='vlan-01' default='yes'>
  </portgroup>
  <portgroup name='vlan-02'>
    <vlan>
      <tag id='2'/>
    </vlan>
  </portgroup>
  <portgroup name='vlan-03'>
    <vlan>
      <tag id='3'/>
    </vlan>
  </portgroup>
  <portgroup name='vlan-all'>
    <vlan trunk='yes'>
      <tag id='2'/>
      <tag id='3'/>
    </vlan>
  </portgroup>
</network>
```

You would use that libvirt network definition in conjunction with a domain configuration like this:

``` xml
<interface type='network'>
  <mac address='11:22:33:44:55:66'/>
  <source network='ovs-network' portgroup='vlan-02'/>
</interface>
```

I just tested this on Ubuntu 12.04.1 with libvirt 0.10.2, and _it worked just fine._ I think the problems I experienced in my earlier testing were all related to incorrect XML configurations. Some other write-ups of the libvirt-OVS integration seem to imply that you need to use the `profileid` parameter to link the domain XML configuration and the network XML configuration, but my testing seems to show that the real link is the portgroup name.

I haven't yet tested this on earlier versions of libvirt, but I can confirm that it works with Ubuntu 12.04 using manually compiled versions of libvirt 0.10.2 and libvirt 1.0.0.

If anyone has any additional information to share, please speak up in the comments.


[1]: {{< relref "2012-11-07-using-vlans-with-ovs-and-libvirt.md" >}}
