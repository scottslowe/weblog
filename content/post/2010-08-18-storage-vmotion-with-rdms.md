---
author: slowe
categories: Explanation
comments: true
date: 2010-08-18T14:52:21Z
slug: storage-vmotion-with-rdms
tags:
- Storage
- Virtualization
- VMware
- vSphere
title: Storage vMotion with RDMs
url: /2010/08/18/storage-vmotion-with-rdms/
wordpress_id: 2023
---

I recently had a colleague contact me with a question about raw device mappings (RDMs) and Storage vMotion. This colleague was trying to perform a Storage vMotion operation on a VM that also had a RDM attached and was running into a problem where the operation was failing because the destination datastore did not have sufficient space. In this case, the free space was less than the size of the mapped raw LUN, and this colleague couldn't perform a Storage vMotion as a result. The colleague was surprised; he didn't expect that behavior.

This behavior struck me as odd and unexpected, so I started digging in and doing some testing. I did all my testing on vSphere 4.0 GA code (no updates), thinking that this was probably "worst case"; if anything, the updates would likely resolve any potential issues with RDMs and Storage vMotion. I used a VM with a 20GB VMDK and a 50GB mapped raw LUN, using a virtual mode RDM.

I performed a few Storage vMotion operations and everything seemed fine; I couldn't recreate the same behavior. Sure, the Datastore Browser shows the VMDK pointer file for the RDM to be the same size as the backing LUN (50GB, in my case), but I couldn't seem to make vCenter Server balk when migrating the VMDK. I tried several times with datastores of various sizes, including a datastore that had less free space than the size of the mapped raw LUN. Then I noticed something: my 20GB VMDK was thin provisioned. Ah, perhaps that was causing part of the problem. So I performed a Storage vMotion to a larger datastore, selecting "Thick Format" during the process to inflate the VMDK to full size.

Now, had I gone back and carefully re-read the table on page 197 of the _vSphere Basic Administration Guide_ (available in PDF [here](http://www.vmware.com/pdf/vsphere4/r40/vsp_40_admin_guide.pdf)), I would have remembered that selecting "Thick Format" with a virtual mode RDM automatically converts the RDM to a virtual disk. But I didn't, and so the RDM was converted into a virtual disk. Subsequent Storage vMotion attempts with the newly-converted virtual disk now produced warnings and errors about available disk space, just as my colleague had seen. Fortunately, this was just a lab environment, so no harm was done. But what if this had been production data?

So here's the key message I want to convey with this blog post: when you are performing Storage vMotion operations on a VM with at least one RDM and you want/need to convert your virtual disks from thick to thin (or vice versa) during the migration, you need to perform two (2) separate Storage vMotion operations:

1. First, you'll migrate _only_ the virtual disks attached to the VM. You'll use the Advanced button when selecting the datastore so that you can leave the RDM alone for this migration. You'll then select the appropriate format ("Thin Provisioned Format" or "Thick Format") for the virtual disks on the target datastore and proceed with the Storage vMotion operation.

2. When the first Storage vMotion concludes, you'll then perform a second Storage vMotion operation to migrate _only_ the RDMs. Again, you'll use the Advanced button when selecting the datastore and choose to move only the RDM. For format, select "Same Format as Source", as this is the only option that preserves the RDM as an RDM. If you are migrating a virtual mode RDM and choose _either_ of the other two options, your RDM is converted to a virtual disk and cannot be converted back.

At this point, your RDM-equipped VM has been migrated to a new datastore, the virtual disks have been converted from thick to thin (or vice versa), and your RDMs have been preserved as RDMs.

Anyone have any other little gotchas like this about RDMs or Storage vMotion they want to share? The comments are open and I'd love to hear any other suggestions or tips from the readers.
