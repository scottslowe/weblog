---
author: slowe
categories: Tutorial
comments: true
date: 2012-10-19T10:21:02Z
slug: link-aggregation-and-lacp-with-open-vswitch
tags:
- Interoperability
- Networking
- OSS
- OVS
- Virtualization
title: Link Aggregation and LACP with Open vSwitch
url: /2012/10/19/link-aggregation-and-lacp-with-open-vswitch/
wordpress_id: 2893
---

In this post, I'm going to show you how to use link aggregation (via the Link Aggregation Control Protocol, or LACP) with Open vSwitch (OVS). First, though, let's cover some basics.

In the virtualization space, it's extremely common to want to use multiple physical network connections in your hypervisor hosts to support guest (virtual machine) traffic. The problem is that modern-day networking is---for now---largely constrained by the presence of Spanning Tree Protocol (STP), which limits the use of multiple connections between network devices (especially switches). Since most hypervisors have some form of virtual switch to support guest traffic, and since users don't want to be constrained by STP the hypervisors have had to find workarounds.

VMware works around STP by causing their virtual switches to operate in what is called "end-host mode," meaning that the virtual switch does not participate in STP (newer versions of vSphere can, in fact, block STP BPDUs from being emitted), the virtual switch does not forward frames received on one uplink back out another uplink, and traffic from VMs is statically assigned (pinned) to an uplink. (This behavior is, of course, configurable.) Because of these default behaviors, users in VMware environments simply connect multiple links to their hosts and off they go.

Other environments behave differently. Environments using Open vSwitch (OVS), for example, need to use other methods to work around the presence of STP, especially considering that OVS is more a full-featured virtual switch than the standard VMware vSwitch. In most cases, the workaround involves the use of link aggregation; specifically, the use of Link Aggregation Control Protocol (LACP), a standardized protocol that allows devices to automatically negotiate the configuration and use of link aggregates comprised of multiple physical links.

Now that you have the background, let's dive into the details of how to make this work. These instructions on using LACP with OVS do make a few assumptions:

1. First, I assume that OVS is already installed and working.

2. I assume that the management traffic to/from your host is _not_ running through OVS, and thus won't be interrupted by any configurations you do here. If this is not the case, and you do have management traffic running through OVS, you might want to exercise some additional caution to ensure you don't accidentally cut your connectivity to the host.

3. I assume that you know how to configure your physical switch(es) to support LACP on the links coming in from OVS. The configuration will vary from switch vendor to switch vendor; refer to your vendor's documentation for details.

This post was written using Ubuntu 12.04.1 LTS and Open vSwitch 1.4.0 (installed using `apt-get` directly from the Precise Pangolin repositories). The use of a different Linux distribution and/or a different version of OVS might make this process slightly different.

The first step is to add a bridge (substitute your desired bridge name for `ovsbr1` in the following command):

    ovs-vsctl add-br ovsbr1

Once the bridge is established, then you'll need to create a bond. This is the actual link aggregate on OVS. The syntax for adding a bond looks something like this:

    ovs-vsctl add-bond <bridge name> <bond name> <list of interfaces>

So, if you wanted to add a bond to `ovsbr1` using physical interfaces `eth1` and `eth3`, your command would look something like this:

    ovs-vsctl add-bond ovsbr1 bond0 eth1 eth3

However, there's a problem with this configuration: by default, LACP isn't enabled on a bond. To fix this, you have two options.

1. Change the command use to create the bond, so that LACP is enabled when the bond is created.

2. Enable LACP after the bond is created.

For option #1, you'll simply append `lacp=active` to the command to create the bond, like so:

    ovs-vsctl add-bond ovsbr1 bond0 eth1 eth3 lacp=active

For option #2, you'd use `ovs-vsctl set` to modify the properties of the bond. Here's an example:

    ovs-vsctl set port bond0 lacp=active

Once the bond is created and LACP is enabled, you can check the configuration and/or status of the bond. Assuming that you've already configured your physical switch correctly, your bond should be working and passing traffic. You can use this command to see the status of the bond:

    ovs-appctl bond/show <bond name>

The output from that command will look something like this:

    bond_mode: balance-slb
    bond-hash-algorithm: balance-slb
    bond-hash-basis: 0
    updelay: 0 ms
    downdelay: 0 ms
    next rebalance: 6415 ms
    lacp_negotiated: true
    
    slave eth4: enabled
        active slave
        may_enable: true
    
    slave eth3: enabled
        may_enable: true
    
    slave eth1: enabled
        may_enable: true
    
    slave eth2: enabled
        may_enable: true

This command will show more detailed LACP-specific information:

    ovs-appctl lacp/show <bond name>

This command returns a great deal of information; here's a quick snippet:

    ---- bond0 ----
        status: active negotiated
        sys_id: 00:22:19:bd:db:dd
        sys_priority: 65534
        aggregation key: 4
        lacp_time: fast
    
    slave: eth1: current attached
        port_id: 4
        port_priority: 65535
    
        actor sys_id: 00:22:19:bd:db:dd
        actor sys_priority: 65534
        actor port_id: 4
        actor port_priority: 65535
        actor key: 4
        actor state: activity timeout aggregation synchronized collecting<br></br>distributing
    
        partner sys_id: 00:12:f2:cc:6d:40
        partner sys_priority: 1
        partner port_id: 12
        partner port_priority: 1
        partner key: 10000
        partner state: activity aggregation synchronized collecting<br></br>distributing

You can also use this command to view the configuration details of the bond:

    ovs-vsctl list port bond0

The output from this command will look something like this:

    _uuid               : ae7eb7ca-e3e0-4166-bcfb-4348071799e0
    bond_downdelay      : 0
    bond_fake_iface     : false
    bond_mode           : []
    bond_updelay        : 0
    external_ids        : {}
    fake_bridge         : false
    interfaces          : [9963381b-6a7d-4a8f-acf8-86150361530e,<br></br>bee2df86-ed14-456b-8f3a-25fb00fa6040, daf5ac51-4135-4e3c-a937-c62dfc4b5e9f,<br></br>fcd2d6ef-9a18-452a-9a79-1c97e5a95ef2]
    lacp                : active
    mac                 : []
    name                : "bond0"
    other_config        : {lacp-time=fast}
    qos                 : []
    statistics          : {}
    status              : {}
    tag                 : []
    trunks              : []
    vlan_mode           : []

In learning how to use LACP with OVS, I found [this article](http://brezular.com/2011/12/04/openvswitch-playing-with-bonding-on-openvswitch/) to be extremely helpful.

If you have questions, or have additional information to share with me and/or other readers, please speak up in the comments. Thanks!
