---
author: slowe
categories: Tutorial
comments: true
date: 2008-04-09T15:31:20Z
slug: keeping-thin-vmdks-using-netapp-snaprestore
tags:
- ESX
- NetApp
- NFS
- Snapshots
- Storage
- Virtualization
- VMware
title: Keeping Thin VMDKs Using NetApp SnapRestore
url: /2008/04/09/keeping-thin-vmdks-using-netapp-snaprestore/
wordpress_id: 680
---

A short while ago, I discussed that VMDKs on NFS may start out thin provisioned, but will [lose that thin provisioned status over time][1]. Operations like cloning and Storage VMotion will cause these thin provisioned disks to become thick (fully provisioned) disks, and you lose one of the benefits of running VMware on NFS.

Fortunately, if you're running a NetApp storage system as your NFS server, you can preserve the thin provisioned status of these VMDKs by leveraging NetApp's single file SnapRestore functionality. This article describes how that works.

There's a couple of caveats here:

1. This technique only helps with making new VMs from an existing VM. SnapRestore won't help preserve thin provisioned status after a Storage VMotion operation.

2. This isn't integrated with VirtualCenter, so you won't be able to take advantage of VirtualCenter's integration with Sysprep and such.

As a result of #2 above, then, you'll need to first prepare your source VM by running Sysprep inside the VM (assuming it is a Windows-based VM) and then allowing Sysprep to shut down the VM. Once that's accomplished, then you can proceed.

The first step is to take a snapshot of the volume containing the already prepared VM:

	snap create <vol-name> <snapshot-name>

Next, create a new VM in VirtualCenter, but **_do not create a virtual disk for the VM._** This will create the VM configuration and associated files and the directory on the NFS datastore.

Third, run a SnapRestore operation to restore both the .vmdk file and the -flat.vmdk files. You have to restore both in order for this to work; keep in mind that the .vmdk file is just a header file and the -flat.vmdk is the actual disk file. The commands would look something like this:

	snap restore -t file -s <snapshot-name> -r <new filename and path> <original filename and path>

As an example, let's say you had a VM named template01 and you wanted to clone the disks for template01 to a new VM called newvm01, and these are stored on a volume called nfsvol. After you've run Sysprep on template01, taken the Snapshot and called it base_snapshot, and created newvm01 without a virtual disk, you'd run this command:

	snap restore -t file -s base_snapshot -r /vol/nfsvol/newvm01/newvm01.vmdk /vol/nfsvol/template01/template01.vmdk

That would restore the .vmdk (header) file; then you'd restore the actual virtual disk file:

	snap restore -t file -s base_snapshot -r /vol/nfsvol/newvm01/newvm01-flat.vmdk /vol/nfsvol/template01/template01-flat.vmdk

Once this process is complete---and it may take some time depending upon the size of the files being restored---you should see both the `.vmdk` and the `-flat.vmdk` files listed in the Datastore Browser.

"But wait a minute, Scott," you say. "The -flat.vmdk no longer looks thin provisioned. You lied! This process doesn't work."

Indeed, it will look like it is no longer thin provisioned. Trust me; there's one more step and then all will make sense. If you log into the ESX Server and open the `.vmdk` file in vi, you'll see that it references the _old file name_ of the `-flat.vmdk`. Edit that to reflect the new, restored file name, save your changes, and go back to the Datastore Broswer again. Refresh the display, and all should be well.

Why does the Datastore Browser work that way? Beats me. You'll also find that running an `ls -la` on an NFS datastore from the Service Console will show you `-flat.vmdk` files that appear to be thick provisioned. The _only_ way I've found to see if they are thin provisioned is to use the Datastore Browser. It's a VMware thing, I suppose.

The last and final step is to edit the settings for the VM you created earlier and add the new virtual disk to that VM. Then you can boot up that VM and proceed with whatever customization steps, if any, are needed.

In the near future I plan to test another possible method of preserving the VMDK's thin provisioned status, a method that is storage agnostic. Look for details of that testing here.

[1]: {{< relref "2008-03-31-only-thin-provisioned-in-the-beginning.md" >}}
