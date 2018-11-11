---
author: slowe
categories: Tutorial
comments: true
date: 2006-12-30T11:56:21Z
slug: recovering-data-inside-vms-using-netapp-snapshots
tags:
- ESX
- NetApp
- ONTAP
- SAN
- Snapshot
- Snapshots
- Storage
- Virtualization
- VMware
title: Recovering Data Inside VMs Using NetApp Snapshots
url: /2006/12/30/recovering-data-inside-vms-using-netapp-snapshots/
wordpress_id: 394
---

[Network Appliance](http://www.netapp.com/) [Snapshots](http://www.netapp.com/products/software/snapshot.html)---point-in-time copies of a file system that can be created almost instantaneously and which generally require much smaller amounts of storage to keep---are an integral part of NetApp's value over other storage systems. These snapshots make it far easier and quicker to recover from data loss or corruption than a tape backup system.

But how do we go about recovering individual files from a snapshot when those files are stored in a virtual disk (VMDK) file used by a VM? After all, [VMware](http://www.vmware.com/) proponents tout the encapsulation property of virtualization as a benefit: "One file to back up and you get a backup of your entire server!" Fortunately, there's a way to continue to reap the benefits of encapsulation while still allowing for the ability to recover individual files from a snapshot of the VM's virtual disk file. Here's how.

The trick here is to take advantage of _LUN cloning_, a feature on the NetApp storage systems that allows you to take a snapshot---which is a read-only point-in-time copy of the file system---and create a clone, which is a read-write point-in-time copy of the file system. This clone takes only seconds to create, like the snapshot on which it is based, and requires only enough storage to store the changed blocks, i.e., the "deltas" between the clone and the original. We can then present that clone back to [VMware ESX Server](http://www.vmware.com/products/vi/esx/) to manipulate in whatever way we see fit.

There are three parts to this process. First, we configure ESX Server to recognize snapshot LUNs on the SAN (this is a one-time configuration change). Then, we take the snapshot on the NetApp storage system, create a LUN clone from the snapshot, and present that LUN clone back to the ESX servers. Finally, we manipulate the LUN clone within ESX in order to retrieve the specific data we need.

### Enable Resignaturing on ESX Server

In the ESX SAN Configuration Guide (found [here on VMware's site](http://www.vmware.com/pdf/vi3_esx_san_cfg.pdf)), there is this blurb about resignaturing:

>VMFS volume resignaturing allows you to make a hardware snapshot of a VMFS volume and access that snapshot from an ESX Server system.

This is the functionality that allows us to use LUN clones on the NetApp storage system in ESX Server. Without this functionality, the LUN clones aren't properly recognized by ESX Server and can't be utilized to allow us to perform data recovery.

To enable VMFS volume resignaturing, set the `LVM.EnableResignature` option to 1 (on). This option can be set in VirtualCenter using these steps:

1. Set the ESX Server host for which you want to enable VMFS volume resignaturing.

2. Go to the Configuration tab for that host, then select Advanced Settings.

3. Change the LVM.EnableResignature to 1 (on). The default is off.

After this option is set, you'll be able to present LUN clones (or other hardware snapshots) to ESX Server and it will recognize them as such.

Now we're ready to move to the NetApp storage system.

### Taking a Snapshot and Making a LUN Clone

By default, snapshots are already enabled and scheduled, so unless you've modified the configuration, the NetApp storage system is already taking snapshots of the volumes that hold the LUNs where the VMware VMFS partitions (and thus the VMDK virtual disk files) are stored.

We can view the list of snapshots using `snap list vol_name`, where "vol_name" is the name of the volume. The output will look something like this:

```text
%/used    %/total     date          name
--------  ----------  ------------  --------
0% ( 0%)    0% ( 0%)  Dec 30 08:00  hourly.0
1% ( 0%)    0% ( 0%)  Dec 30 00:00  nightly.0
1% ( 0%)    0% ( 0%)  Dec 29 20:00  hourly.1
1% ( 0%)    0% ( 0%)  Dec 29 16:00  hourly.2
1% ( 0%)    0% ( 0%)  Dec 29 12:00  hourly.3
2% ( 1%)    0% ( 0%)  Dec 29 08:00  hourly.4
2% ( 0%)    0% ( 0%)  Dec 29 00:00  nightly.1
3% ( 0%)    0% ( 0%)  Dec 28 20:00  hourly.5
```

Now, we can make a LUN clone from one of these snapshots and map it to an igroup (this would normally all be on a single line, but I've wrapped it here for readability):

	lun clone create /vol/vol_name/lun0_clone -b /vol/vol_name/lun0_vmfs nightly.1  
	lun map /vol/vol_name/lun0_clone igroup_name 0

The LUN clone has now been created and presented back to the igroup named "igroup\_name" as LUN ID 0. A rescan of the storage adapters in ESX Server (iSCSI was being used in this case) will now show the LUN clone as "snap-00000001-lun0_vmfs" (the number will change depending upon how many snapshot LUNs have been presented to the server farm). Now that we have access to the VMFS, we can do any number of things:

* We can create a new VM with the same configuration as the original VM and boot it up to recover data from the VM in that manner (be cautious of networking issues, such as duplicate IP addresses). You'll just need to select the existing VMDK (or VMDKs, if there are more than one) on the snapshot VMFS LUN instead of creating a new virtual disk file when creating the VM.

* We can attach the VMDK(s) to an existing VM running the same operating system (or an operating system that will read the file system used inside the VMDKs in question) and then browse the file system to retrieve data stored inside the VM.

* We could shut down the original VM and boot up the clone VM instead. This might be helpful if you needed to recover data but also needed network connectivity, or if the two VMs couldn't be running at the same time. (In theory, this might work for [Microsoft Exchange](http://www.microsoft.com/exchange/default.mspx), if you aren't using [SnapManager for Exchange](http://www.netapp.com/products/software/snapmanager-exchange.html).)

As you can see, this allows us to take full advantage of encapsulating the server in the VMDK file(s) but also allows us to retrieve individual files or groups of files from a snapshot of the VMDK file(s).

In future articles, I'll touch on restoring entire VMs using NetApp snapshots, as well as talk about getting consistent snapshots of the VMs.

### Other Information

This process was performed on a Network Appliance FAS810 running Data ONTAP 7.1.1.1 and servers running VMware ESX Server (both 3.0.0 and 3.0.1) with the software iSCSI initiator. VMs running Windows Server 2003 R2 were used for testing.
