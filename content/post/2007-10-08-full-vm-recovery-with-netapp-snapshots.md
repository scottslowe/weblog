---
author: slowe
categories: Tutorial
comments: true
date: 2007-10-08T16:17:35Z
slug: full-vm-recovery-with-netapp-snapshots
tags:
- ESX
- iSCSI
- NAS
- NetApp
- NFS
- SAN
- Virtualization
- VMware
- Storage
title: Full VM Recovery with NetApp Snapshots
url: /2007/10/08/full-vm-recovery-with-netapp-snapshots/
wordpress_id: 558
---

I guess I'm on a bit of a NetApp kick this week. After discussing (or perhaps revisiting) the idea of recovering files inside VMs using NetApp Snapshots (first [here late last year][1], then again [here][2]), I wanted to take a closer look at full VM recovery using NetApp Snapshots.

First of all, it should go without saying that you should never use any of the procedures I'm describing here without first testing them yourself. While they worked fine for me, they may not work fine for you. Don't just assume they will! Do the due diligence and test it in your environment first; you'll be glad you did.

Second, before using NetApp Snapshots to recover VM data (file-level or full VM), be sure you are getting good, consistent Snapshots. The [Network Appliance Technical Reports Library](http://www.netapp.com/library/tr/) has a number of excellent articles on this subject; I'll defer you there for more information.

I'll break this article into two sections, one for block-level storage (I'm using iSCSI, but the process should be almost identical for Fibre Channel) and one for NAS/NFS. Please note that I'm not focusing so much on the specific steps that are required as I am on general concepts and any gotchas that may arise during the process.

## Full VM Recovery using Block Storage

To recover a full VM using block-level storage, a number of steps have to be taken:

1. Create a LUN clone (or a FlexClone) of the original LUN based on a Snapshot.

2. Enable resignaturing on the ESX host(s) that will need to see the cloned LUN.

3. Mount the cloned LUN(s) on the ESX host(s) and copy the appropriate VM files from the clone to the production LUN.

For the first two steps, I'll refer you back to one of my first articles on VMware data recovery with Snapshots, which has more information on the necessary commands and settings.

For the third step, you'll need to login to the Service Console (typically via SSH) and copy the desired VM(s)--and all their files---from the cloned datastore to the production datastore, overwriting whatever is in the destination (you typically wouldn't need to recover a full VM unless the production VM was hosed, right?). Once the file(s) have been copied back over to the production datastore, dismount the cloned datastore and destroy it.

You should now be able to boot up your VM at the state it was in at the time of the Snapshot used to recover it. Unless the Snapshot was a cold Snapshot (taken while the VM was powered off), the VM will perform a file system check (chkdsk or fsck) when it boots up.

## Full VM Recovery using NFS

The procedure for recovering full VMs when using NFS is even easier:

1. Using an NFS client, mount the NFS export and navigate to the hidden ".snapshot" directory.

2. In the ".snapshot" directory, find the Snapshot from which you wish to recover the VM.

3. Copy that VM's files (the entire folder) out of the ".snapshot" directory into the production filesystem, replacing the current contents (again, this assumes that what's in the production filesystem is no good, else why would you be recovering a full VM?).

4. Unmount the NFS export from your NFS client.

The recovered VM should now boot and be back to the point in time at which the Snapshot was taken. Again, unless the Snapshot was a cold Snapshot, the VM will likely perform a file system check upon boot. This is normal and not unexpected.

I suppose you could even do this second procedure from a CIFS client, assuming that CIFS and NFS were both configured on the storage system and an appropriate CIFS share existed. (Please note that I've never tried this, so I can't tell you what the results might be.) In that case, use the "~snapshot" directory instead of ".snapshot".

And that's it---there you have two ways of recovering entire VMs using Network Appliance Snapshots. As always, feel free to hit me up in the comments with any questions, thoughts, corrections, or rants (just keep the rants on-topic, please!). Thanks for reading!

[1]: {{< relref "2006-12-30-recovering-data-inside-vms-using-netapp-snapshots.md" >}}
[2]: {{< relref "2007-10-08-vm-file-level-recovery-with-netapp-snapshots.md" >}}
