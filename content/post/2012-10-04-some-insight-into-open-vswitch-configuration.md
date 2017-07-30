---
author: slowe
categories: Explanation
comments: true
date: 2012-10-04T09:28:18Z
slug: some-insight-into-open-vswitch-configuration
tags:
- Networking
- OSS
- OVS
- Virtualization
title: Some Insight into Open vSwitch Configuration
url: /2012/10/04/some-insight-into-open-vswitch-configuration/
wordpress_id: 2872
---

As you may already know, I've been working with [Open vSwitch (OVS)](http://openvswitch.org/) for a few weeks now, trying to wrap my head around how this open source project works. One thing that I really struggled with---and still struggle with, to a certain extent---is a lack of user-friendly documentation. While there are a few posts that provide some basic instructions, I haven't found any good articles that provide a bit more depth and more explanation. Maybe it's just me, but I like to know _why_ I'm typing certain commands, and _how_ those commands work.

What _is_ relatively well-documented are tasks like creating a bridge, adding ports to a bridge, or creating a bond. These tasks use commands like `ovs-vsctl add-br` or `ovs-vsctl add-port`. These commands are pretty easy to understand, reasonably well-documented in the help screens and the manpages, and provide decent error messages back to the user if the syntax is wrong. What _isn't_ quite so well-documented are tasks like VLANs or LACP, and that's where I was struggling.

(OK, rant over.)

Anyway, I think that I've finally made some progress, and I wanted to share what I've found with you. (At some point in the near future, I intend to post some "intro to OVS basics" posts to help others that might be learning OVS for the first time like me.)

So, let's say that you want to set the VLANs that are allowed across an OVS port. By default, all OVS ports are VLAN trunks, but based on my experience you still need to set the VLANs that are allowed across the trunk before they actually act like trunks. If you're familiar with Cisco switches, think of this process as using the `switchport trunk allowed vlans` command.

To set the VLAN trunks for a given port, use this command:

    ovs-vsctl set port <port name> trunks=<list of VLAN IDs>

Obviously, you'll want to substitute the correct port name and VLAN IDs in that command. If you're unsure of the port name to use, use `ovs-vsctl show` to review the OVS configuration and determine which port(s) to configure.

If you want to configure an OVS port as a VLAN access port, use this command:

    ovs-vsctl set port <port name> tag=<VLAN ID>

Again, it should go without stating, but you'll want to substitute the correct values for your environment in the command above.

Here's one more example before I provide some explanation. Let's say that you have a bond (a NIC team or a link aggregate) and you want to enable LACP on that bond. You'd run this command:

    ovs-vsctl set port <port name> lacp=active

The "port name" in this case would be something like bond0, which is a bond you've created using `ovs-vsctl add-bond` command. Your physical switch must be properly configured as well in order to support LACP on the appropriate physical switch ports.

What are these commands doing, exactly? This is the part that I couldn't find documented (well) anywhere. I saw lots of references to `ovs-vsctl` parameters like `set interface` or `set port`, but no clear explanation of what these commands were doing, or why. Almost every single example I saw also used these commands during the process of creating a bridge, bond, port, or interface (not afterward). What if you needed to modify the values after the object is created? Do you have to delete the object and recreate it?

Oddly enough, it was [this post](http://blog.sflow.com/2010/05/configuring-open-vswitch.html) (which has _nothing_ to do with VLANs, trunks, or LACP, but instead focuses on sFlow) that sparked my understanding. I was reading the post on how to configure OVS for sFlow while also reviewing the manpage for `ovs-vsctl` when I had the epiphany: these objects (bridge, port, bond, interface) are _tables_ in the OVSDB, so you need to use the OVSDB-related parameters for `ovs-vsctl` in order to modify their properties.

Looking at the `ovs-vsctl` manpage (or the `--help` screen), you can see that there are several DB-related commands. Here's the generic form:

    ovs-vsctl <command> <table name> <record name> <setting=value>

In this generic command, `<command>` would be something like set, get, or list, and the `<table name>` would be replaced by a specific OVSDB table. For example, one such table is `port`. Let's plug a specific command and a specific table into the generic form:

    ovs-vsctl set port <record name> <setting=value>

(Did something just click with you?)

We could continue plugging specific items into the generic form to arrive at a command like this:

    ovs-vsctl set port bond0 trunks=10,20,30,40,50

The trick, of course, is knowing what values to substitute into the command to manipulate the OVS database in the right way. Fortunately, there are a couple of commands that can help.

To see all the OVS bridges and their settings, use this command:

    ovs-vsctl list bridge

To see all the OVS ports and their settings, use this command:

    ovs-vsctl list port

Finally, to see all the OVS interfaces and their settings, use this command:

    ovs-vsctl list interface

You can add a specific record to the above commands; for example, to see the settings for a port named `bond0`:

    ovs-vsctl list port bond0

This will show you the settings that are available for that particular record; you can then use `ovs-vsctl set` as described earlier to set the value for a setting. This is how you configure VLAN trunks (by setting the value of the trunks setting for a particular port) or enable LACP (by setting the value of the LACP setting for a particular port). These commands can be run when a record is created, like this:

    ovs-vsctl add-bond br0 bond0 eth0 eth1 lacp=active trunks=10,11,12

Or you can run the commands after the record/object is created, like this:

    ovs-vsctl set port bond0 lacp=active trunks=10,11,12

Hopefully, this additional information and insight---which seems so simple now that I understand it---will prove helpful to others.

If there are errors or inaccuracies in my information, please speak up in the comments and correct me. This will also help other readers. All courteous comments are welcome!
