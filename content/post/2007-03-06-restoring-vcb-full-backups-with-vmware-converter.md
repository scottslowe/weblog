---
author: slowe
categories: Explanation
comments: true
date: 2007-03-06T11:37:35Z
slug: restoring-vcb-full-backups-with-vmware-converter
tags:
- Backup
- ESX
- Storage
- Virtualization
- VMware
title: Restoring VCB Full Backups with VMware Converter
url: /2007/03/06/restoring-vcb-full-backups-with-vmware-converter/
wordpress_id: 425
---

When performing full VM backups with [VMware Consolidated Backup](http://www.vmware.com/products/vi/consolidated_backup.html) (VCB), the end result of the backup operation is a copy of the VMs VMDK files, in 2GB blocks. This format is similar to the format used by [VMware Server](http://www.vmware.com/products/server/) and [VMware Workstation](http://www.vmware.com/products/ws/), VMware's hosted virtualization products. Given that [VMware Converter](http://www.vmware.com/products/converter/) can convert VMs between ESX Server and the hosted virtualization products, I thought, "Why not use VMware Converter to restore VCB backups?"

So, with that question in mind, I set to see if it would work. After conducting some tests, I have good news and bad news. The bad news first: it doesn't work without some manual massaging of the VCB backup. When you attempt to laod the VM's files into VMware Converter, you receive an error message that VMware Converter "can't determine the guest OS".

Fortunately, the good news is that it isn't hard to make it work. Thanks to this [VMTN forums thread](http://www.vmware.com/community/message.jspa?messageID=588351), we can see that the process is actually pretty straightforward:

1. Edit the `.vmx` file to remove the "-NNNNNN" suffix that is automatically appended to the names of any VMDK files referenced in the `.vmx` file. Make sure the VMDK filenames referenced in the `.vmx` file are indeed unique (they should be).

2. Rename the actual VMDK files by removing the "scsiN-N-N-" prefix from the filenames. You'll want to ensure that the filenames match the filenames referenced in the `.vmx` file.

3. Edit the VMDK index file (not the -sNNN files). This is just a text file that references the rest of the files that comprise the virtual hard disk. You'll need to edit the filenames referenced in this file to ensure that they match the names of the actual "-sNNN" files on the disk.

That should be it. After making those changes, VMware Converter should read the VM files without giving the "can't determine guest OS" error, and then will let you select your final destination (which would typically be VirtualCenter).

Note that this will create a new VM, rather than restoring over the existing VM. As a result, this may be most applicable when you need to recover individual files within a VM and aren't performing file-level backups using VCB. By using VMware Converter and creating a new VM, you can boot the VM up, get the files that are needed, then shut the restored VM down and blow it away. This allows you to have the restore functionality of file-level backups but the speed of full VM backups.
