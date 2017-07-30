---
author: slowe
categories: Education
comments: true
date: 2009-06-23T15:59:45Z
slug: new-user-networking-config-guide
tags:
- CLI
- ESX
- Networking
- Virtualization
- VMware
title: New User's Guide to Configuring VMware ESX Networking via CLI
url: /2009/06/23/new-user-networking-config-guide/
wordpress_id: 1411
---

A lot of the content on this site is oriented toward VMware ESX/ESXi users who have a pretty fair amount of experience. As I was working with some customers today, though, I realized that there really isn't much content on this site for new users. That's about to change. As the first in a series of posts, here's some new user information on creating vSwitches and port groups in VMware ESX using the command-line interface (CLI).

For new users who are seeking a thorough explanation of how VMware ESX networking functions, I'll recommend a series of articles by Ken Cline titled [The Great vSwitch Debate](http://kensvirtualreality.wordpress.com/2009/03/29/the-great-vswitch-debate-part-1/). Ken goes into a great level of detail. Go read that, then you can come back here.

Before I get started it's important to understand that, for the most part, the information in this article applies _only_ to VMware ESX. VMware ESXi doesn't have a Linux-based Service Console like VMware ESX, and therefore doesn't have a readily-accessible CLI from which to run these sorts of commands. There is a remote CLI available, which I'll discuss in a future post, but for now I'll focus only on VMware ESX.

The majority of all the networking configuration you will need to perform on VMware ESX boils down to just a couple commands:

* `esxcfg-vswitch`: You will use this command to manipulate virtual switches (vSwitches) and port groups.

* `esxcfg-nics`: You will use this command to view (and potentially manipulate) the physical network interface cards (NICs) in the VMware ESX host.

Configuring VMware ESX networking boils down to a couple basic tasks:

1. Creating, configuring, and deleting vSwitches

2. Creating, configuring, and deleting port groups

I'll start with creating, configuring, and deleting vSwitches.

## Creating, Configuring, and Deleting vSwitches

You'll primarily use the `esxcfg-vswitch` command for the majority of these tasks. Unless I specifically indicate otherwise, all the commands, parameters, and arguments are case-sensitive.

To create a vSwitch, use this command:

	esxcfg-vswitch -a <vSwitch Name>

To link a physical NIC to a vSwitch---which is necessary in order for the vSwitch to pass traffic onto the physical network or to receive traffic from the physical network---use this command:

	esxcfg-vswitch -L <Physical NIC> <vSwitch Name>

In the event you don't have information on the physical NICs, you can use this command to list the physical NICs (that's a lowercase L in the command):

	esxcfg-nics -l

Conversely, if you need to unlink (remove) a physical NIC from a vSwitch, use this command:

	esxcfg-vswitch -U <Physical NIC> <vSwitch Name>

To change the Maximum Transmission Unit (MTU) size on a vSwitch, use this command:

	esxcfg-vswitch -m <MTU size> <vSwitch Name>

To delete a vSwitch, use this command:

	esxcfg-vswitch -d <vSwitch Name>

## Creating, Configuring, and Deleting Port Groups

As with virtual switches, the `esxcfg-vswitch` is the command you will use to work with port groups. Once again, unless I specifically indicate otherwise, all the commands, parameters, and arguments are case-sensitive.

To create a port group, use this command:

	esxcfg-vswitch -A <Port Group Name> <vSwitch Name>

To set the VLAN ID for a port group, use this command:

	esxcfg-vswitch -v <VLAN ID> -p <Port Group Name> <vSwitch Name>

To delete a port group, use this command:

	esxcfg-vswitch -D <Port Group Name> <vSwitch Name>

To view the current list of vSwitches, port groups, and uplinks, use this command (that's a lowercase L in the command):

	esxcfg-vswitch -l

There are more networking-related tasks that you can perform from the CLI, but for a new user these commands should handle the lion's share of all the networking configuration. Good luck!
