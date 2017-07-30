---
author: slowe
categories: Tutorial
comments: true
date: 2008-04-22T14:00:02Z
slug: esx-server-ip-storage-and-jumbo-frames
tags:
- ESX
- iSCSI
- NetApp
- NFS
- Storage
- Virtualization
- VMware
title: ESX Server, IP Storage, and Jumbo Frames
url: /2008/04/22/esx-server-ip-storage-and-jumbo-frames/
wordpress_id: 688
---

With the release of VMware Infrastructure version 3.5, VMware added support for [jumbo frames](http://en.wikipedia.org/wiki/Jumbo_frame). Although the [documentation](http://www.vmware.com/support/vi3/doc/whatsnew_esx35_vc25.html) states that jumbo frames "are not supported for NAS and iSCSI traffic", jumbo frames for NFS and iSCSI does actually work. Here's some information on getting it working.

## How I Tested

Keep in mind that this is not an "officially supported" configuration (see the section on the "Official" Support Statement below), so **_use at your own risk._** I will not be held responsible if you blow up your production environment trying to make jumbo frames work.

Here's how I tested the use of jumbo frames for software iSCSI and NFS datastores:

* For the physical switch infrastructure, I used a Cisco Catalyst 3560G running Cisco IOS version 12.2(25)SEB4.

* For the physical server hardware, I used a group of HP ProLiant DL385 G2 servers with dual-core AMD Opteron processors and a quad-port PCIe Intel Gigabit Ethernet NIC.

* For the storage system, I used a NetApp FAS940 running Data ONTAP 7.2.4.

The exact commands and/or procedures may be different for you depending upon the hardware and/or software versions that you're running in your environment. Keep that in mind.

## Configuring the Physical Switch

Fortunately for me, the Cisco Catalyst 3560G does indeed support jumbo frames. (Naturally, you'll want to ensure that your switch supports jumbo frames.) Jumbo frames are not, however, enabled by default; they must be enabled using the following command in global configuration mode:

	system mtu jumbo 9000

Note that 9000 bytes seems to be the generally accepted size for jumbo frames, so that's what I used.

After running this command, _you must reboot the switch._ The change doesn't take effect until a reload. Fortunately, IOS reminds you of this after you enter the command. Once the switch has rebooted, you can verify the MTU setting with this command:

	show system mtu

This should report that the system jumbo MTU size is 9000 bytes, confirming that the switch is ready for jumbo frames. Now we're prepared to configure the storage system.

## Configuring the Storage System

Using FilerView, increasing the MTU on the appropriate network interfaces to 9000 bytes is as simple as going to Network > Manage Interfaces and then clicking the Modify link for the interface to be changed. Set the "MTU size" to 9000 (from the default of 1500), click Apply, and you're ready to roll.

You can verify the settings in FilerView using Network > Manage Interfaces > Show All Interface Details, or by using the `ifconfig -a` command from a Data ONTAP command prompt.

## Configuring ESX Server

There is no GUI in VirtualCenter for configuring jumbo frames; all of the configuration must be done from a command line on the ESX server itself. There are two basic steps:

1. Configure the MTU on the vSwitch.
2. Create a VMkernel interface with the correct MTU.

First, we need to set the MTU for the vSwitch. This is pretty easily accomplished using esxcfg-vswitch:

	esxcfg-vswitch -m 9000 vSwitch1

A quick run of `esxcfg-vswitch -l` (that's a lowercase L) will show the vSwitch's MTU is now 9000; in addition, `esxcfg-nics -l` (again, a lowercase L) will show the MTU for the NICs linked to that vSwitch are now set to 9000 as well.

Second, we need to create a VMkernel interface. This step is a bit more complicated, because we need to have a port group in place already, and that port group needs to be on the vSwitch whose MTU we set previously:

	esxcfg-vmknic -a -i 172.16.1.1 -n 255.255.0.0 -m 9000 IPStorage

This creates a port group called IPStorage on vSwitch1--the vSwitch whose MTU was previously set to 9000--and then creates a VMkernel port with an MTU of 9000 on that port group. Be sure to use an IP address that is appropriate for your network when creating the VMkernel interface.

To test that everything is working so far, use the `vmkping` command:

	vmkping -s 9000 172.16.1.200

Clearly, you'll want to substitute the IP address of your storage system in that command.

That's it! From here you should be able to easily add an NFS datastore or connect to an iSCSI LUN using jumbo frames from the ESX server.

## "Official" Support Statement

Officially, jumbo frames are only supported by VMware for use by virtual machines. Technically, VMware does not support the use of jumbo frames for the software iSCSI initiator or for use with NFS datastores. At least, that's my understanding.

So, feel free to tinker around with jumbo frames for IP-based storage, and when VMware adds official support for it in the future---I can't imagine why they wouldn't---then you'll be able to hit the ground running with the configuration steps necessary to make it work.
