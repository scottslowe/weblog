---
author: slowe
categories: Education
comments: true
date: 2010-08-23T15:00:00Z
slug: new-users-guide-to-configuring-vmware-esxi-networking-via-cli
tags:
- CLI
- ESXi
- Networking
- Virtualization
- VMware
- vSphere
title: New User's Guide to Configuring VMware ESXi Networking via CLI
url: /2010/08/23/new-users-guide-to-configuring-vmware-esxi-networking-via-cli/
wordpress_id: 2039
---

This is one article in a series of articles focused toward new users. Some other New User's Guide articles include:

* [New User's Guide to Configuring VMware ESX Networking via CLI][1]

* [New User's Guide to Configuring Cisco MDS Zones via CLI][2]

* [New User's Guide to Managing Cisco MDS Zones via CLI][3]

This particular article is a follow-up of sorts to the first article listed above. While that article focused on virtual networking with VMware ESX, this article focuses on virtual networking with VMware ESXi. Given that VMware's stated focus is on VMware ESXi moving forward, I thought this article would be helpful and timely.

For new users who are seeking a thorough explanation of how VMware ESX/ESXi networking functions, I'll recommend a series of articles by Ken Cline titled [The Great vSwitch Debate](http://kensvirtualreality.wordpress.com/2009/03/29/the-great-vswitch-debate-part-1/). Ken goes into a great level of detail. Go read that, then you can come back here.

All of the commands presented in this article were testing using VMware vSphere 4.1. The environment consisted of hosts running VMware ESXi 4.1 being managed by VMware vCenter Server 4.1. For CLI access, I used the vSphere Management Assistant (vMA) virtual appliance, deployed via OVF.

The majority of all the networking configuration you will need to perform on VMware ESXi boils down to just a few commands:

* _vicfg-vswitch:_ You will use this command to manipulate virtual switches (vSwitches) and port groups.

* _vicfg-vmknic:_ You will use this command to create, modify, or delete VMkernel NICs on the VMware ESXi hosts.

* _vicfg-nics:_ You will use this command to view (and potentially manipulate) the physical network interface cards (NICs) in a VMware ESXi host.

The tasks that you'll actually perform using this commands are pretty straightforward:

1. Creating, configuring, and deleting vSwitches

2. Creating, configuring, and deleting port groups

3. Creating, configuring, and deleting VMkernel NICs

I'll start with a few prerequisites that are necessary due to the fact that you are using a remote CLI to access the VMware ESXi hosts.

As you can see from the list above, all the commands you're going to use are the `vicfg-*` commands. All of these commands have some standard parameters they require in addition to the task-specific parameters. To make things a bit simpler for you, I'll recommend that you set persistent values (persistent for the current vMA session, at least) to simplify the commands later. Here are the values I recommend you establish:

* First, set the value of the VI_SERVER variable to be the fully qualified domain name of the vCenter Server computer. Use the bash `export` command to set this variable. The command looks like `export VI_SERVER=vcenter-server.domain.com`.

	Setting this variable now means that none of the `vicfg-*` commands will need to have this parameter specified. Since it's likely that you'll consistently work with one specific instance of vCenter Server, then this is a pretty safe variable to set.

* In the absence of using Active Directory integration (which is a far cleaner choice, but one which we'll reserve for a future article), set the VI_USERNAME variable to the name of the user account you'll use to authenticate against vCenter Server. Again, use the `export` command as outlined in the previous bullet.

Now that you have some basics established, I'll move on to creating, configuring, and deleting vSwitches.

## Creating, Configuring, and Deleting vSwitches

You'll use the `vicfg-vswitch` command for the majority of these tasks. Unless I specifically indicate otherwise, all the commands, parameters, and arguments are case-sensitive. For all these `vicfg-*` commands, you will get prompted for the password to the user account you defined when you set the value of the VI_USERNAME variable.

To create a vSwitch, use this command:

	vicfg-vswitch -h <ESXi hostname> -a <vSwitch Name>

To link a physical NIC to a vSwitch---which is necessary in order for the vSwitch to pass traffic onto the physical network or to receive traffic from the physical network---use this command:

	vicfg-vswitch -h <ESXi hostname> -L <Physical NIC> <vSwitch Name>

In the event you don't have information on the physical NICs, you can use this command to list the physical NICs (that's a lowercase L in the command):

	vicfg-nics -h <ESXi hostname> -l

Conversely, if you need to unlink (remove) a physical NIC from a vSwitch, use this command:

	vicfg-vswitch -h <ESXi hostname> -U <Physical NIC> <vSwitch Name>

To change the Maximum Transmission Unit (MTU) size on a vSwitch, use this command:

	vicfg-vswitch -h <ESXi hostname> -m <MTU size> <vSwitch Name>

To delete a vSwitch, use this command:

	vicfg-vswitch -h <ESXi hostname> -d <vSwitch Name>

## Creating, Configuring, and Deleting Port Groups

As with virtual switches, the `vicfg-vswitch` is the command you will use to work with port groups. Once again, unless I specifically indicate otherwise, all the commands, parameters, and arguments are case-sensitive.

To create a port group, use this command:

	vicfg-vswitch -h <ESXi hostname> -A <Port Group Name> <vSwitch Name>

To set the VLAN ID for a port group, use this command:

	vicfg-vswitch -h <ESXi hostname> -v <VLAN ID> -p <Port Group Name> <vSwitch Name>

To delete a port group, use this command:

	vicfg-vswitch -h <ESXi hostname> -D <Port Group Name> <vSwitch Name>

To view the current list of vSwitches, port groups, and uplinks, use this command (that's a lowercase L in the command):

	vicfg-vswitch -h <ESXi hostname> -l

## Creating, Configuring, and Deleting VMkernel NICs

To work with ESXi's VMkernel NICs, you'll primarily use the `vicfg-vmknic` command. As in the previous sections, all commands are case-sensitive unless I specifically indicate otherwise, and all commands assume you've defined the VI_SERVER and VI_USERNAME variables.

To create a new VMkernel NIC, use this command:

	vicfg-vmknic -h <ESXi hostname> -a -i <VMkernel NIC IP address> -n <Subnet mask> <Port group>

To delete a VMkernel NIC, use this command:

	vicfg-vmknic -h <ESXi hostname> -d <Port group>

To enable vMotion on an already-created VMkernel NIC:

	vicfg-vmknic -h <ESXi hostname> -E <Port group>

There are more networking-related tasks that you can perform from the CLI, but for a new user these commands should handle the lion's share of all the networking configuration. Good luck with your ESXi environment!

[1]: {{< relref "2009-06-23-new-user-networking-config-guide.md" >}}
[2]: {{< relref "2009-08-24-new-users-guide-to-configuring-cisco-mds-zones-via-cli.md" >}}
[3]: {{< relref "2009-10-20-new-users-guide-to-managing-cisco-mds-zones-via-cli.md" >}}
