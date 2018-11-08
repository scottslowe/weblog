---
author: slowe
categories: Tutorial
comments: true
date: 2012-10-19T15:23:56Z
slug: vlans-with-open-vswitch-fake-bridges
tags:
- Interoperability
- Networking
- OSS
- OVS
- Virtualization
- VLAN
title: VLANs with Open vSwitch Fake Bridges
url: /2012/10/19/vlans-with-open-vswitch-fake-bridges/
wordpress_id: 2898
---

In other posts, I've (briefly) talked about how to [configure][1] Open vSwitch (OVS) for use with VLANs. If you know the port to which a guest is connected, you can configure that particular port as a VLAN trunk like this:

    ovs-vsctl set port <port name> trunks=10,11,12

This configuration would pass the VLAN tags for VLANs 10, 11, and 12 all the way up to the guest, where---assuming the OS installed in the guest has VLAN support---you could configure network connectivity appropriately.

Alternately, if you know the port to which a particular guest is connected, you could configure that port as a VLAN access port with a command like this:

    ovs-vsctl set port <port name> tag=15

This command makes the guest a member of VLAN 15, much like the use of the `switchport access vlan 15` command on a Cisco switch.

These commands are all well and good, but there's a couple problems here:

1. First, you must know which port corresponds to which guest domain. Thus far, I have been unable to determine what set of commands will help me (you) establish the mapping between ports/interfaces and guest domains. (If you know how, please speak up in the comments!)

2. Second, even if you do know which port corresponds to which guest, the settings are ephemeral. That is, when you power off the guest, the port---and its associated configuration---goes away. You'd then need to reapply the configuration to the port when you start the guest domain again.

Clearly, this is not ideal. Fortunately, there is a workaround---a couple of them, actually. One workaround is to add OVS and VLAN support to [libvirt](http://libvirt.org) (something that is actually mentioned [here](http://www.siliconloons.com/?p=305)). This is a great idea---but it doesn't work just yet. On some systems (I use Ubuntu 12.04.1 LTS with libvirt 0.10.2), the libvirt-OVS-VLAN integration causes an error. A patch has been submitted to libvirt to fix this problem (great work Kyle!), but it hasn't (yet) made it into a release.

Without OVS/VLAN support in libvirt, we have only one other workaround: OVS fake bridges. OVS fake bridges look and act like a bridge, but are tied to a particular VLAN ID. (I haven't seen/found a way to use a fake bridge to do VLAN trunking up to a guest domain. Anyone else know how?) In this post, I'm going to show you how to use OVS fake bridges to add VLAN support to your OVS environment.

This post was written using Ubuntu 12.04.1 LTS with Open vSwitch 1.4.0 (straight out of the Precise Pangolin repositories). Please note that the commands might be slightly different on other distributions or with other versions of OVS.

To create a fake bridge, you'll use a modified form of the `ovs-vsctl add-br` command. The command is so subtly different that I missed it quite a few times when reading through the documentation for `ovs-vsctl`. Here's the command you'll need:

    ovs-vsctl add-br <fake bridge> <parent bridge> <VLAN>

Let's look at an example. Suppose you had an existing OVS bridge named ovsbr0, and you wanted to add a fake bridge to support VLAN 100. You would use this command:

    ovs-vsctl add-br vlan100 ovsbr0 100

When you create (or edit) a guest domain, you'll assign it to the new fake bridge (named `vlan100` in this example). So, looking at the libvirt XML code for a guest domain, it might look something like this:

```xml
<interface type='bridge'>
  <mac address='11:22:33:aa:bb:cc'/>
  <source bridge='vlan100'/>
  <address type='pci' domain='0x0000' bus='0x00' slot='0x03' function='0x0'/>
</interface>
```

Naturally, you could also create a libvirt virtual network that corresponds to the fake bridge as well. (I'll likely post a separate article around that idea.)

Then, when you powered up the guest domain and ran `ovs-vsctl show`, you'd see something like this:

    Bridge "ovsbr0"
        Port "bond0"
            Interface "eth1"
            Interface "eth2"
        Port "ovsbr0"
            Interface "ovsbr0"
                type: internal
        Port "vnet0"
            tag: 100
            Interface "vnet0"
        Port "vlan100"
            tag: 100
            Interface "vlan100"
                type: internal

Note that the guest domain's port/interface are automatically given the fake bridge's VLAN tag, without any further interaction/configuration required by the user or administrator. Much better!

Assuming you're using fake bridges (and if you're using OVS and VLANs, I'm not sure how you wouldn't be), there are a couple other commands you might find helpful as well:

* The `ovs-vsctl br-to-vlan` command will print the VLAN ID for a given bridge. If the bridge is a real bridge, the command returns 0; if the bridge is a fake bridge, it returns the VLAN ID.

* The `ovs-vsctl br-to-parent` command returns the parent bridge for a given fake bridge. If the specified bridge is a real bridge, it returns the real bridge.

Using fake bridges with link aggregation is also possible, as you can see from the snippet of OVS configuration above. More information on OVS with link aggregation is available [here][2].

I hope this information is useful. OVS is a really powerful piece of software, and I'm enjoying learning more about it and how to use it. If anyone has any additional information, please feel free to speak up in the comments. All courteous comments are welcome!

[1]: {{< relref "2012-10-04-some-insight-into-open-vswitch-configuration.md" >}}
[2]: {{< relref "2012-10-19-link-aggregation-and-lacp-with-open-vswitch.md" >}}
