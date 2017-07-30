---
author: slowe
categories: Information
comments: true
date: 2009-06-29T10:30:33Z
slug: snapshot-issue-with-vmware-data-recovery
tags:
- Backup
- Snapshot
- Virtualization
- VMware
title: Snapshot Issue with VMware Data Recovery
url: /2009/06/29/snapshot-issue-with-vmware-data-recovery/
wordpress_id: 1427
---

Reader Kyle Ross shared with me a potential issue with VMware's new backup product, VMware Data Recovery. Others within the VMware blogging scene have also covered this, but I wanted to mention it as well so that others didn't run into the problem. Here's Kyle's write-up:

>I was made aware of a serious (in my opinion) bug with VDR during a call with VMware support that I haven't seen discussed anywhere. This is an internally known issue that causes snapshots to build up on VM's that are members of VDR backup jobs.
>
>During the backup process a new snapshot is created and VDR updates the snapshot descriptor file (`vm_name-000001.vmdk`) to mark the snapshot as un-removable. The bug is introduced when the backup process completes, it fails to mark the snapshot as removable causing them to remain.
>
>The tricky part of the problem is that the snapshots are not visible through the vSphere Client, nor are they listed in apps like 'RVTools' that use the VMware CLI to gather data. They could potentially be listed in the new datastore views but I didn't think to look there before I resolved it in my environment. I ran across them by logging into the service console and running the following command to list all the delta files on the datastores attached to the server.
>
>`find /vmfs/volumes/ -name \*delta\*`
>
>In my environment I noticed numerous VMs with multi-gigabyte delta files that I couldn't account for via snapshots listed in the GUI. Here is the solution I was given by VMware. Via the Service Console, browse to the location of the VMDK files for the affected VM. Run this command to identify the descriptors that need to be corrected, replacing 'virtual_machine_name' with the actual name of the VM.
>
>`grep I ddb.dele *virtual_machine_name*-000???.vmdk`
>
>This command will quickly identify the delta files that are marked as non-deletable. The workaround is to edit the affected VMDK descriptor files and change "ddb.deletable" from "false" to "true". You will probably also need to edit the root VMDK file and change this field as well, otherwise you may be left with one open snapshot. Note that due to a change in how ESX 4 performs file locking, you will probably need to SSH into the host that is currently running the VM to edit these files. Once you have edited all the files, create a new snapshot for the VM either via the GUI or command line. Then issue the "Delete All" snapshots command to force ESX to combine all the files and close all the visible and hidden snapshots.

As soon as more information is available, I'll post it here. If any other readers have more information to share, please speak up in the comments.
