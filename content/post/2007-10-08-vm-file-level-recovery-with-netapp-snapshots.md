---
author: slowe
categories: Tutorial
comments: true
date: 2007-10-08T16:37:44Z
slug: vm-file-level-recovery-with-netapp-snapshots
tags:
- ESX
- NetApp
- NFS
- Snapshot
- Snapshots
- Storage
- Virtualization
- VMware
title: VM File-Level Recovery with NetApp Snapshots
url: /2007/10/08/vm-file-level-recovery-with-netapp-snapshots/
wordpress_id: 557
---

Last year, I wrote an article about [using NetApp Snapshots and LUN clones][1] to enable the recovery on individual files within a VM. This time around, I'd like to have a quick at that same process, but this time using NFS instead of block-level storage.

As I mentioned [a couple of weeks ago][2], NFS is getting more and more attention as a key storage enabler for Virtual Infrastructure implementations. I do still plan to conduct some tests of my own between iSCSI and NFS. (Since they are both IP-based storage protocols, I figure that makes the playing field as level as possible.) In any case, with regards to file-level recovery within VMs, NFS does possess at least one advantage.

Using any sort of clones (LUN clones or FlexClones) within VI3 currently requires resignaturing enabled, or else the ESX Servers don't even **see** the clones. While enabling resignaturing is not difficult (can be done via the command line or via VirtualCenter), it is not the default configuration and VMware appears not to recommend it (per the _SAN Configuration Guide_, pages 112 through 115). With NFS, it's only necessary to create a FlexClone and set up a new NFS mount; no other configuration is required.

By the same token, using NFS for file-level recovery within VMs also has one key disadvantage: LUN clones are free, whereas the use of FlexClone requires a license.

With these advantages and disadvantages in mind, let's have a look at the what the process would look like to recover files inside VMs using NFS for VM storage with NetApp Snapshots.

First, we'd review the list of available Snapshots using the `snap list <volname>` command. For example, for a volume named "nfs\_volume1", the command would be `snap list nfs_volume1`. The output of that command would look something like this:

```
  %/used       %/total  date          name  
----------  ----------  ------------  --------  
  0% ( 0%)    0% ( 0%)  Oct 08 12:00  hourly.0  
  0% ( 0%)    0% ( 0%)  Oct 08 08:00  hourly.1  
  0% ( 0%)    0% ( 0%)  Oct 08 00:00  nightly.0  
  0% ( 0%)    0% ( 0%)  Oct 07 20:00  hourly.2  
  0% ( 0%)    0% ( 0%)  Oct 07 16:00  hourly.3  
  0% ( 0%)    0% ( 0%)  Oct 07 12:00  hourly.4  
  0% ( 0%)    0% ( 0%)  Oct 07 08:00  hourly.5  
  0% ( 0%)    0% ( 0%)  Oct 07 00:00  nightly.1`
```

Once we identify the Snapshot that contains the data we need to recover (based on the date/time of the Snapshot), we create a FlexClone using that Snapshot as its backing:

	vol clone create nfs_volume1_clone -s file -b nfs_volume1 nightly.0

This creates a FlexClone named nfs\_volume1\_clone based on the nightly.0 Snapshot of the volume nfs\_volume1. If you immediately run the `exportfs` command, you'll see that the new clone is already shared via NFS, too.

From here, the process is pretty straightforward:

1. Create a new NFS datastore within VirtualCenter, using the new NFS mount as the destination. This makes the data inside the FlexClone visible to the existing VMs.

2. Add one of the VMDKs on the cloned NFS datastore to an existing VM as an additional hard drive. You should be able to do this on the fly without shutting down the VM.

3. Extract the files you need and place them back where you want them.

When you're done recovering files, the clean-up process looks like this:

1. Remove the VMDK(s) from the VM to which it/they was/were added.

2. Remove the NFS datastore from VirtualCenter.

3. Destroy the FlexClone using the `vol offline` and `vol destroy` commands.

Overall, this process is rather similar to the technique described using LUN clones, although a bit simpler because resignaturing is not required.

[1]: {{< relref "2006-12-30-recovering-data-inside-vms-using-netapp-snapshots.md" >}}
[2]: {{< relref "2007-09-21-nfs-for-vmware-storage.md" >}}
