---
author: slowe
categories: Tutorial
comments: true
date: 2012-11-07T09:00:00Z
slug: using-vlans-with-ovs-and-libvirt
tags:
- Interoperability
- Libvirt
- Linux
- Networking
- OSS
- OVS
- Virtualization
- VLAN
title: Using VLANs with OVS and libvirt
url: /2012/11/07/using-vlans-with-ovs-and-libvirt/
wordpress_id: 2934
---

In previous posts, I've shown you how to use [Open vSwitch (OVS) with VLANs through fake bridges][1], as well as how to [wrap libvirt virtual networks around OVS fake bridges][2]. Both of these techniques are acceptable for configuring VLANs with OVS, but in this post I want to talk about using VLANs with OVS via a greater level of [libvirt](http://libvirt.org) integration. This has been talked about [elsewhere](http://www.siliconloons.com/?p=305), but I wasn't able to make it work until libvirt 1.0.0 was released. (**Update:** I was able to make it work with an earlier version. See [here][3].)

First, let's recap what we know so far. If you know the port to which a particular domain (guest VM) is connected, you can configure that particular port as a VLAN trunk like this:

	ovs-vsctl set port <port name> trunks=10,11,12

This configuration would pass the VLAN tags for VLANs 10, 11, and 12 all the way up to the domain, where---assuming the OS installed in the domain has VLAN support---you could configure network connectivity appropriately. (I hope to have a blog post up on this soon.)

Along the same lines, if you know the port to which a particular domain is connected, you could configure that port as a VLAN access port with a command like this:

	ovs-vsctl set port <port name> tag=15

This command makes the domain a member of VLAN 15, much like the use of the `switchport access vlan 15` command on a Cisco switch. (I probably don't need to state that this isn't the _only_ way---see the other OVS/VLAN related posts above for more techniques to put a domain into a particular VLAN.)

These commands work perfectly fine and are all well and good, but there's a problem here---the VLAN information _isn't_ contained in the domain configuration. Instead, it's in OVS, attached to an ephemeral port---meaning that when the domain is shut down, the port **and the associated configuration** disappears. What I'm going to show you in this post is how to use VLANs with OVS in conjunction with libvirt for persistent VLAN configurations.

This document was written using Ubuntu 12.04.1 LTS and Open vSwitch 1.4.0 (installed straight from the Precise Pangolin repositories using `apt-get`). Libvirt was compiled manually (see instructions [here][4]). Due to some bugs, it appears you need at least version 1.0.0 of libvirt. Although the Silicon Loons article I referenced earlier mentions an earlier version of libvirt, I was not able to make it work until the 1.0.0 release. Your mileage may vary, of course---I freely admit that I might have been doing something wrong in my earlier testing.

To make VLANs work with OVS and libvirt, two things are necessary:

1. First, you must define a libvirt virtual network that contains the necessary portgroup definitions.

2. Second, you must include the portgroup reference to the virtual network in the domain (guest VM) configuration.

Let's look at each of these steps.

## Creating the Virtual Network

The easiest way I've found to create the virtual network is to craft the network XML definition manually, then import it into libvirt using `virsh net-define`.

Here's some sample XML code (I'll break down the relevant parts after the code):

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

The key takeaways from this snippet of XML are:

1. First, note that the OVS bridge is specified as the target bridge in the `<bridge name=...>` element. You'll need to edit this as necessary to make your specific OVS configuration. For example, in my configuration, `ovsbr0` refers to a bridge that handles only host management traffic.

2. Second, note the `<portgroup name=...>` element. This is where the "magic" happens. Note that you can have no VLAN element (as in the `vlan-01` portgroup), a VLAN tag (as in the `vlan-10` or `vlan-20` portgroups), or a set of VLAN tags to pass as a trunk (as in the `vlan-all` portgroup).

Once you've got the network definition in the libvirt XML format, you can import that configuration with `virsh net-define <XML filename>`. (Prepend this command with `sudo` if necessary.)

After it is imported, use `virsh net-start <network name>` to start the libvirt virtual network. If you make changes to the virtual network, such as adding or removing portgroups, be sure to restart the virtual network using `virsh net-destroy <network name>` followed by `virsh net-start <network name>`.

Now that the virtual network is defined, we can move on to creating the domain configuration.

## Configuring the Domain Networking

As far as I'm aware, to include the appropriate network definitions in the domain XML configuration, you'll have to edit the domain XML manually.

Here's the relevant snippet of domain XML configuration:

``` xml
<interface type='network'>
  <mac address='11:22:33:44:55:66'/>
  <source network='ovs-network' portgroup='vlan-02'/>
</interface>
```

You'll likely have more configuration entries in your domain configuration, but the important one is the `<source network=...>` element, where you'll specify both the name of the network you created **as well as** the name of the portgroup to which this domain should be attached.

With this configuration in place, when you start the domain, it will pass the necessary parameters to OVS to apply the desired VLAN configuration automatically. In other words, once you define the desired configuration in the domain XML, it's maintained persistently inside the domain XML (instead of on the ephemeral port in OVS), re-applied anytime the domain is started.

## Verifying the Configuration

Once the appropriate configuration is in place, you can see the OVS configuration created by libvirt when a domain is started by simply using `ovs-vsctl show` or---for more detailed information---`ovs-vsctl list port <port name>`. Of particular interest when using `ovs-vsctl list port <port name>` are the `tag` and/or `trunks` values; these are where VLAN configurations are applied.

## Summary

In this post, I've shown you how to create libvirt virtual networks that integrate with OVS to provide persistent VLAN configurations for domains connected to an OVS bridge. The key benefit that arises from this configuration is that you longer need to know to which OVS port a given domain is connected. Because the VLAN configuration is stored _with_ the domain and applied to OVS automatically when the domain is started, you can be assured that a domain will always be attached to the correct VLAN when it starts.

As usual, I encourage your feedback on this article. If you have questions, thoughts, corrections, or clarifications, you are invited to speak up in the comments below.


[1]: {{< relref "2012-10-19-vlans-with-open-vswitch-fake-bridges.md" >}}
[2]: {{< relref "2012-10-22-wrapping-libvirt-virtual-networks-around-open-vswitch-fake-bridges.md" >}}
[3]: {{< relref "2012-11-12-libvirt-ovs-integration-revisited.md" >}}
[4]: {{< relref "2012-11-05-compiling-libvirt-1-0-0-on-ubuntu-12-04-and-12-10.md" >}}
