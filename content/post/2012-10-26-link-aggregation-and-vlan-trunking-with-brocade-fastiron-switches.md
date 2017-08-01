---
author: slowe
categories: Tutorial
comments: true
date: 2012-10-26T10:20:00Z
slug: link-aggregation-and-vlan-trunking-with-brocade-fastiron-switches
tags:
- Interoperability
- Networking
- OVS
- Virtualization
- VLAN
- VMware
- vSphere
- CLI
title: Link Aggregation and VLAN Trunking with Brocade FastIron Switches
url: /2012/10/26/link-aggregation-and-vlan-trunking-with-brocade-fastiron-switches/
wordpress_id: 2907
---

In this post, I'll be sharing with you information on how to do link aggregation (with LACP) and VLAN trunking on a Brocade FastIron switch with both VMware vSphere as well as Open vSwitch (OVS).

Throughout the majority of my career, my networking experience has been centered around Cisco's products. You can easily tell that from looking at the articles I've published here. However, I've recently had the opportunity to spend some time working with a Brocade FastIron switch (a 48-port FastIron Edge X, specifically, running software version 7.2), and I wanted to write-up what I've learned about how to do link aggregation and VLAN trunking in conjunction with both VMware vSphere as well as OVS.

## Configuring Link Aggregation with LACP

When researching how to do link aggregation on a Brocade FastIron, I came across a number of different articles suggesting two different ways to configure link aggregation (ultimately I followed the information provided in [this article](http://community.brocade.com/docs/DOC-1984) and [this article](http://community.brocade.com/docs/DOC-1993)). I think that the difference in the configuration comes down to whether or not you want to use LACP, but I'm not completely sure. (If you're a Brocade/Foundry expert, feel free to weigh in.)

To configure a link aggregate using LACP, use these commands:

* You'll use the `link-aggregate configure key <unique key>` command to identify which interfaces may participate in a given link aggregate. The key must range from 10000 to 65535, and has to be unique for each group of interfaces in a link aggregate bundle. The switch uses the key to identify which ports may be a part of a link aggregate.

* You'll use the `link-aggregate active` command to indicate the use of LACP for link aggregation configuration and negotiation.

For example, if you wanted to configure port 10 on a switch for link aggregation, the commands would look something like this:

    switch(config)# interface ethernet 10
    switch(config-if-e1000-10)# link-aggregate configure key 10000
    switch(config-if-e1000-10)# link-aggregate active

For each additional port that should also belong in this same link aggregate bundle, you would repeat these commands and use the same key value. As I mentioned earlier, the identical key value is what tells the switch which interfaces belong in the same bundle.

Configuring the virtualization host is pretty straightforward from here:

* If you are using vSphere, note that you'll need to use vSphere 5.1 and a vSphere Distributed Switch (VDS) in order to use LACP. In order to use LACP, you'll need to set your teaming policy to "Route based on IP hash," and then you must enable LACP in the settings for the uplink group. Chris Wahl has a nice write-up [here](http://wahlnetwork.com/2012/10/15/using-lacp-with-a-vsphere-distributed-switch-5-1/), including a list of the caveats of using LACP with vSphere. VMware also has [a VMware KB article](http://kb.vmware.com/kb/2034277) on the topic.

* If you are using OVS, you can follow the instructions I provided in this post on [link aggregation and LACP with Open vSwitch][1].

## Configuring VLANs

Although VLANs are (generally) interoperable between different switch vendors due to the broad adoption of the 802.1Q standard, the details of each vendor's implementation of VLANs is just different enough to make life difficult. In this particular case, since I learned Cisco's VLAN implementation first, Brocade's VLAN implementation on the FastIron Edge X series switches seemed rather odd. I'm sure that had I learned Brocade's implementation first, then Cisco's version would seem odd.

In any case, the commands you use for VLANs are as follows:

* To create a VLAN, use the `vlan <VLAN identifier>` command.

* To add a port to that VLAN, so that traffic across that port is tagged for the specified VLAN, use the `tagged ethernet <interface>` command.

* To add a range of ports to a VLAN, use the `tagged ethernet <start interface> to <end interface>` command.

* To allow a port to carry both untagged (native, or default VLAN) and tagged traffic, you must use the `dual-mode` command. Otherwise, a port carries only untagged _or_ tagged traffic. (This was a key difference in Brocade's VLAN implementation that threw me off at first.)

So, if you wanted to create VLAN 30, add Ethernet interface 24 to that VLAN, and configure the interface to carry both tagged and untagged traffic, the commands would look something like this:

    switch(config)# vlan 30 name server-subnet
    switch(config-vlan-30)# tagged ethernet 24
    switch(config-vlan-30)# interface ethernet 24
    switch(config-if-e1000-24)# dual-mode

Once the VLANs are created and the interfaces are added to the VLANs, configuring the virtualization hosts is---once again---pretty straightforward:

* On vSphere, you must create a port group (or distributed port group) for each VLAN, and specify the VLAN ID in the port group configuration. Any port group without a VLAN ID specified will receive untagged traffic. I wrote an article in 2007 (it's a bit dated, but still applicable) about [VMware port groups and the native VLAN][2]. (The native VLAN is Cisco terminology for the untagged VLAN.)

* On OVS, you must either [create OVS fake bridges for each VLAN][3] or (when it's working) use libvirt integration with OVS. You can [wrap libvirt networks around OVS fake bridges][4] as well.

I hope this information is useful to someone. If anyone has any corrections or clarifications, I encourage you to add your information to the comments on this post.

[1]: {{< relref "2012-10-19-link-aggregation-and-lacp-with-open-vswitch.md" >}}
[2]: {{< relref "2007-11-13-esx-server-and-the-native-vlan.md" >}}
[3]: {{< relref "2012-10-19-vlans-with-open-vswitch-fake-bridges.md" >}}
[4]: {{< relref "2012-10-22-wrapping-libvirt-virtual-networks-around-open-vswitch-fake-bridges.md" >}}
