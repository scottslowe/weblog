---
author: slowe
categories: Tutorial
comments: true
date: 2013-05-28T09:00:00Z
slug: vlan-trunking-to-guest-domains-with-open-vswitch
tags:
- Linux
- Networking
- OVS
- Ubuntu
- VLAN
title: VLAN Trunking to Guest Domains with Open vSwitch
url: /2013/05/28/vlan-trunking-to-guest-domains-with-open-vswitch/
wordpress_id: 3191
---

In other articles, I've talked about how to use [Open vSwitch (OVS) with VLANs][1] to place guest domains (VMs) into a particular VLAN. In this article, I want to show you how to pass VLAN tags all the way into the guest domain---in other words, how to do VLAN trunking to guest domains using OVS. To do this, we're going to leverage the OVS-libvirt integration I referenced in this post on [using VLANs with OVS and libvirt][2].

For this to work, you must have an operating system in the guest domain that is capable of recognizing and using the VLAN tags that are being passed to it by OVS. In this article, I'll use Ubuntu 12.04 as the OS in the guest domain. For other operating systems, the commands and/or procedures to configure VLAN support appropriately will probably differ, so keep that in mind.

There are two parts to making this work:

1. Configuring OVS (manually or via libvirt) to pass VLAN tags to the guest OS.

2. Configuring the guest domain's installed OS to take advantage of the VLAN tags being passed up by OVS.

Let's look at each of these parts separately. We'll start with configuring OVS, either manually or via libvirt, to pass the VLAN tags up to the guest domain.

## Configuring OVS to Pass VLAN Tags to the Guest Domain

There are two ways to accomplish this: you can do it manually, or you can do it via OVS integration with recent builds of libvirt.

### Manually Configuring OVS

To configure OVS manually, you would need to:

1. Identify which vnet port you want to configure for VLAN trunking

2. Configure the vnet port to trunk the VLANs.

To identify which vnet port needs to be modified, you'll want to figure out the guest domain interface(s) that is/are connected to the vnet port. You can do this by using this command (substitute the desired vnet port name in place of `vnet0` in the following command):

    ovs-vsctl list interface vnet0

In the output of the command, look for the `external_ids` line; it will contain an entry called "attached-mac", and that represents the MAC address of the interface in the guest domain OS attached to this particular vnet port. You can compare this to the output of `ip addr list` or `ifconfig -a` in Ubuntu to find a matching MAC address in the guest domain. Correlating the two values allows you to determine which guest domain is attached to which vnet port, and then you can modify the correct vnet port appropriately.

You'd modify the vnet port using this command:

    ovs-vsctl set port vnet0 trunks=20,30,40

You'd want to substitute the appropriate values for `vnet0` and the VLAN IDs that you want passed up to the guest domain. Once you've made the change, you can verify the changes using this command (replacing `vnet0` with the correct port):

    ovs-vsctl list port vnet0

Note that if you want the guest domain to receive both untagged (native VLAN) traffic as well as tagged (trunked) traffic, there is an additional setting you must set:

    ovs-vsctl set port vnet0 vlan_mode=native-untagged

With this setting in place, the OS installed into the guest domain will be able to communicate over the untagged (native) VLAN as well as using VLAN tags.

### Using libvirt Integration

If the manual method of configuring OVS seems a bit cumbersome, using the libvirt integration makes it _much_ easier.

Basically, you'll follow the configuration outlined in [this blog post][2] to create a libvirt network that corresponds to an OVS bridge. Here's an example of the XML code to accomplish this task (click [here][gist-1] for an option to download this code snippet):

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

Of particular interest for what we're trying to accomplish here is the very last section, the portgroup named "vlan-all." Note that for this specific portgroup, the `vlan` element has a property that specifies it is a trunk, and then there are multiple `tag` elements that list each VLAN ID that will be trunked across this network into the guest domain.

Using this configuration, when we create the guest domain and specify that it is attached to the network named "vlan-all" (matching the portgroup in the libvirt network definition), libvirt will automatically configure OVS appropriately (it will set the `trunks` value for that domain's OVS port).

**However,** it will not configure the OVS port to allow untagged traffic as well (only tagged traffic will be passed). If you want the guest domain to receive untagged traffic also, you must set the `vlan_mode` value manually as outlined above.

## Configuring the Guest Domain to Use VLAN Tags

Once you've followed the steps outlined above and have OVS configured correctly, then you're ready to configure the OS in the guest domain. Keep in mind that I'm using Ubuntu 12.04 in this post, but you're welcome to use any operating system that supports VLAN tags.

Assuming that eth0 is the interface in the guest domain that is receiving tagged traffic from OVS, this snippet in `/etc/network/interfaces` will create and configure a VLAN interface (click [here][gist-2] for an option to download this snippet):

``` text
auto eth0.20
iface eth0.20 inet static
  vlan-raw-device eth0
  address 192.168.20.200
  netmask 255.255.255.0
  network 192.168.20.0
  broadcast 192.168.20.255
```

Technically, the "raw-vlan-device" line isn't needed because the parent device name is in the name of the VLAN device, but I like to include it for completeness and ease of debugging. (Your mileage may vary, of course.) The number on the end of the eth0 (for example, eth0.20) corresponds to the VLAN ID (VLAN 20, in this case) being passed up by OVS.

You can repeat this configuration for multiple VLAN interfaces.

## Use Case

I'll have to admit that I can't immediately think of some useful use cases for this sort of configuration. At first glance, you might think that it would be useful in situations where you need logical separation, but I think there are better ways than VLANs to accomplish this task (and those ways are probably simpler). I primarily set out to document this in order to better solidify my knowledge of how OVS works and is configured. However, I'd be happy to hear from others on what they think might be interesting or useful use cases for this sort of configuration. Feel free to add your thoughts in the comments below. Courteous comments are always welcome!


[gist-1]: https://gist.github.com/scottslowe/4057683
[gist-2]: https://gist.github.com/scottslowe/5658227
[1]: {{< relref "2012-10-19-vlans-with-open-vswitch-fake-bridges.md" >}}
[2]: {{< relref "2012-11-07-using-vlans-with-ovs-and-libvirt.md" >}}
