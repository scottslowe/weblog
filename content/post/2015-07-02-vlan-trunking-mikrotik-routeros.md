---
author: slowe
categories: Tutorial
comments: true
date: 2015-07-02T09:35:00Z
tags:
- CLI
- Networking
- VLAN
- Linux
title: VLAN Trunking with Mikrotik RouterOS
url: /2015/07/02/vlan-trunking-mikrotik-routeros/
---

In this post, I'm going to show you how to configure VLAN trunking with Mikrotik RouterOS, and along the way provide a brief introduction to this software and some of the functionality it offers. While it is Linux-based, RouterOS operates quite a bit differently than a lot of the other network operating systems with which I've worked, and so I hope that this post will help ease the learning curve a bit for others who decide to take the same path.

## Background

First, let me provide a quick bit of background. I found myself in need of a switch that was both Layer 2/3 capable with both 10/100/1000Mbps ports as well as 10Gbps SFP+ ports. Of course, this was for my home lab, so budget is a concern. I cast out a quick call on Twitter, asking for some recommendations, and a few folks recommended I have a look at RouterBoard/Mikrotik; specifically, the CRS-24G-2S+IN (see [here][link-2] for more details). The specs looked good, the price was reasonable, and several folks expressed their satisfaction with the product, so I bought one.

Upon receiving it, I found myself trying to unravel RouterOS (their Linux-based operating system). Their [wiki][link-3] is fairly helpful, but I found [the CRS examples page][link-1] to be the most helpful in my particular situation. The information I'm going to share here is based on the information in the wiki.

## CLI Concepts

Like some other environments, the RouterOS CLI is based on the idea of _contexts._ In that respect, you can draw some similarities with the EMC VPLEX CLI (see my [introductory post on the VPLEX CLI][xref-1]) or the Cisco UCS CLI. So, for example, there is an `interface` context, a `user` context, etc. Each context contains a set of context-specific commands. When you're in a particular context (sort of like being in a particular directory), the prompt will reflect that, and you can type commands from that context directly. If, however, you need a command from a different context, then you have to preface the command with the full path. If I was in the `user` context and wanted to run a command in the `interface` context, I'd have to type `/interface <command> <command>`. Again, it's a bit like working with a directory structure.

Another thing to note about the RouterOS CLI is the way it handles data structures. To see a list of VLANs in the switch, you'd use the `print` command in the appropriate context. Specifically, in this case, you run this command (I'm including the full path/context just for the sake of completeness):

    /interface ethernet switch vlan print

This would print a tabular list of entries, each with an ID (a unique identifier), the assigned VLAN ID, the ports associated with that VLAN, and various other settings. To edit any of these values, you would use the `edit` command. As an example, let's say you wanted to edit the list of ports associated with a particular VLAN. Once you'd determined the ID of the VLAN (using the output of the `print` command shown previously), you'd use the `edit` command like this:

    /interface ethernet switch vlan edit 1 ports

This would edit the "ports" value of the VLAN whose ID in the list (**not** the VLAN ID) is 1. The generic form of the command is this:

    /interface ethernet switch vlan edit <ID> <value>

I'll try to show some examples of this later in the post, but you'll use this type of command (in different contexts) to modify lots of settings throughout different contexts in RouterOS.

OK, that's enough for some of the CLI concepts; let's look specifically at setting up VLAN trunking with RouterOS.

## Configuring VLAN Trunking

Just so we're all on the same page with regard to terminology, a _VLAN trunk_ is a port that carries multiple VLANs tagged with the assigned VLAN IDs. If you need a port that will carry more than one VLAN, you need a VLAN trunk.

In order to set up a VLAN trunk in RouterOS, you must explicitly tell the switch to tag the appropriate VLANs on that particular port. This is done with the `/interface ethernet switch egress-vlan-tag` command. So, for example, if you wanted to tag VLANs 100, 200, and 300 on port ether20, you'd use this command (I've line-wrapped them with backslashes here, but you would enter them all on a single line):

    /interface ethernet switch egress-vlan-tag add \
    tagged-ports=ether20 vlan-id=100
    /interface ethernet switch egress-vlan-tag add \
    tagged-ports=ether20 vlan-id=200
    /interface ethernet switch egress-vlan-tag add \
    tagged-ports=ether20 vlan-id=300

This command works fine if you haven't already created an egress VLAN tag entry for the VLAN ID specified in the command. If, however, you already have an egress VLAN tag entry---perhaps because you're now adding a port to an existing entry---this command will report an error. This is one of those cases where you need to use the `edit` command, as I described above.

So, you'd do this:

1. You'd use the `/interface ethernet switch egress-vlan-tag print` command (or just the `print` command if you are already in the `/interface ethernet switch egress-vlan-tag` context) to list all the egress VLAN tag entries.
2. From the list, determine the ID (again, **not** the VLAN ID---this is a separate value) of the entry you need to change.
3. Use the `edit` command, like this (you can omit the full context if you're already in that context):

        /interface ethernet switch egress-vlan-tag edit <ID> tagged-ports

    If the ID of the entry is 5 (i.e., it's the fifth entry in the list), you'd run:

        /interface ethernet switch egress-vlan-tag edit 5 tagged-ports

4. In the full-screen text editor, simply add the port names to the comma-delimited list on the screen. Press Ctrl-O to save and quit when you're done (or Ctrl-C if you want to exit without saving any changes).

Using these four steps will allow you to edit the egress VLAN tag entry for a particular VLAN ID, where you can add ports (thus telling RouterOS to tag that VLAN when traffic leaves the switch on that port) or remove ports (thus telling RouterOS to stop tagging traffic with that VLAN ID when it leaves the switch on that port).

To filter (or prune) VLANs---in other words, to specify that a VLAN trunk will only tag a subset of the VLAN IDs defined on the switch---simply don't add an egress VLAN tag entry for that VLAN ID for that port. The example I showed above where I tagged VLANs 100, 200, and 300 on port ether20 is a great example; those are the VLANs tagged on that port, but the switch might also have VLANs 150, 250, and 350 defined. Those VLANs _won't_ be tagged on ether20, and therefore are effectively pruned (or filtered) from that port.

## Configuring Access Ports

Since we're already knee-deep in VLAN concepts with RouterOS, let's just go ahead and talk about VLAN access ports as well. An _access port_ is a port where all traffic is considered to be part of a particular VLAN. The incoming traffic isn't tagged, though, so you have to tell RouterOS how to handle the traffic.

In this case, you have to tell RouterOS to explicitly tag incoming traffic with the appropriate VLAN ID. To do this, you'll use the `/interface ethernet switch ingress-vlan-translation` command. This command essentially re-writes the VLAN ID on the incoming traffic. Since incoming traffic isn't tagged, the existing VLAN ID on incoming traffic is 0.

Let's say you wanted a particular port to be an access port on VLAN 200. You'd use this command (it's line-wrapped here, but you'll want to enter it all on a single line):

    /interface ethernet switch ingress-vlan-translation \
    add ports=ether20 customer-vid=0 new-customer-vid=200 sa-learning=yes

This command tells RouterOS to translate a VLAN ID of 0 on port ether20 to a VLAN ID of 200. The effective result is a port where all traffic is assigned to a particular VLAN---an access port.

Hopefully this information is helpful to someone else out there. RouterOS is actually quite powerful, and exposes a lot of features and functions---although accessing and configuring those features and functions is sometimes more difficult or unintuitive than perhaps it should be.



[link-1]: http://wiki.mikrotik.com/wiki/Manual:CRS_examples
[link-2]: http://routerboard.com/CRS226-24G-2SplusIN
[link-3]: http://wiki.mikrotik.com/wiki/Main_Page
[xref-1]: {{< relref "2010-12-22-an-introduction-to-the-vplex-cli.md" >}}