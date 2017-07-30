---
author: slowe
categories: Tutorial
comments: true
date: 2007-10-03T22:52:52Z
slug: sanrad-configuration-basics
tags:
- iSCSI
- Storage
- Virtualization
title: Sanrad Configuration Basics
url: /2007/10/03/sanrad-configuration-basics/
wordpress_id: 552
---

The [Sanrad V-Switch](http://www.sanrad.com/Products/) is a useful device that offers a number of features, including (but not limited to) storage virtualization, iSCSI-to-FC connectivity, snapshots, and data migration services. It's actually a pretty handy little device. I had the opportunity to spend the day today with a V-Switch 2000, an entry-level model, connected in front of a small Fibre Channel array. I thought it might be handy to include some configuration commands here in the event that someone else needed them (to be honest, the Sanrad documentation is _horrible_).

Overall, the process for configuring a V-Switch looks something like this:

1. Physically connect the V-Switch to the Ethernet network and the Fibre Channel fabric(s).

2. Set the IP configuration for the Ethernet interfaces on the V-Switch.

3. Create an iSCSI portal and an iSCSI target on the V-Switch.

4. Present FC storage to the V-Switch.

5. Create volumes on the V-Switch.

6. Present the volumes as iSCSI LUNs.

And that's it! Once you get used to the interface (I used the command line interface because...well, I prefer the command line to the GUI). Let's walk through those steps real quick.

## Physically Connect the V-Switch

The V-Switch gets connected via Gigabit Ethernet and via Fibre Channel. Physically connecting the V-Switch is as simple as plugging in the Ethernet and Fibre Channel ports to the appropriate switches. It's probably also going to be necessary to modify your Fibre Channel zones as well, so that the V-Switch can see any Fibre Channel targets on the fabric(s).

## Configure IP

The V-Switch comes with two Gigabit Ethernet interfaces. To assign IP address(es) to the interface(s), use the `ip config set` command, like this:

	ip config set -ip 10.2.3.45 -if eth1 -im 255.255.255.0

Verify the IP address has been added using _ip config show_ command, as well as by pinging the new IP address from another system on the same subnet.

By the way, the system comes with a pre-assigned IP address (I believe it is 10.11.12.123) which can't be removed. At least, I couldn't figure out how to remove or change it.

## Create iSCSI Portal and iSCSI Target

Next, we create an iSCSI portal (IP address and listening TCP port) on the assigned IP address:

	iscsi portal create -ip 10.2.3.45 -p 3260

TCP port 3260 is the default, so you could omit that if desired. The `iscsi portal show` command will verify the creation of the iSCSI portal.

Once the iSCSI portal is created, create an iSCSI target with the `iscsi target create` command, like this (I used a backslash to denote line continuation):

	iscsi target create -ta vmtarget \  
	-tn iqn.2000-04.com.sanrad:vswitch01

We can verify the creation of the iSCSI target using `iscsi target show`. At this point, iSCSI initiators will see the iSCSI target, but won't see any LUNs, because we haven't connected and configured the back-end storage yet. That's next.

## Present FC Storage to the V-Switch

The exact procedures and commands here will vary based on the back-end storage array. Create the volumes, disk groups, LUNs, RAID groups, etc., using the standard tools and commands provided by the back-end storage array. Present those storage containers to the V-Switch using the V-Switch's WWNN (which can be displayed using the `fc interface show` command).

Once that's done, the V-Switch should discover it automatically. To show the storage that the V-Switch has discovered, use this command:

	storage show

This will show disks and controllers with very unimaginative (and undescriptive) names like "Stor_10". To keep things logicaly and organized, rename the storage to something that more closely identifies what it is. In this example, we'll identify the storage with a logical drive identifier and an array identifier:

	storage set -s Stor_15 -na LD09-Ar01 -info Log drv 9, Array 1  
	storage set -s Stor_6 -na LD00-Ar00 -info Log drv 0, Array 0

With the storage now recognized by the V-Switch and labeled as something recognizable (and traceable back to the storage array itself), we can create a couple of simple volumes:

	volume create simple -vol simple00 -d LD00-Ar00  
	volume create simple -vol simple05 -d LD05-Ar01

Simple volumes are the building blocks for more complex structures like mirrored volumes or striped volumes. With a couple of simple volumes created, we can create a mirrored volume from these simple volumes:

	volume create mirror -vol mirror01 -ch simple00 -ch simple05

Verify the creation of the volumes (simple or mirrored) with the `volume show` command.

Let's create a few more simple volumes:

	volume create simple -vol simple01 -d LD01-Ar00  
	volume create simple -vol simple02 -d LD02-Ar00  
	volume create simple -vol simple06 -d LD06-Ar01  
	volume create simple -vol simple02 -d LD07-Ar01

Then, using these four simple volumes, let's create a striped volume (this should be entered as a single command; I've added backslashes here to denote line breaks for readability):

	volume create stripe -vol stripe01 -nbc 4 \  
	-sus 64 -ch simple01 -ch simple02 -ch simple06 \  
	-ch simple07

Again, we can use the `volume show` command to list the volumes that have been created. However, our job is not yet done. We have to present these volumes as LUNs to the iSCSI initiators.

## Present iSCSI LUNs

The volumes have been created from back-end storage, but now we need to expose the volumes to iSCSI initiators on the iSCSI target created earlier. We do that with this command:

	volume expose -vol mirror01 -ta vmtarget

This exposes the volume named "mirror01" on the iSCSI target named "vmtarget" (created earlier). That target is accessible on the iSCSI portal also created earlier.

You can verify or show that the volume is exposed as a LUN on the iSCSI target using the `lu show` command.

Assuming that it is properly exposed, then you should be able to now see the exposed LUN via your iSCSI initiator (software or hardware).

The V-Switch also has some other advanced functionality, like subdisks (carving up LUNs from back-end storage into smaller pieces), snapshots, and data replication/migration. I hope to get the opportunity to discuss those features more in the future.
