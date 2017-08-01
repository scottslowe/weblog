---
author: slowe
categories: Explanation
comments: true
date: 2006-12-12T12:36:58Z
slug: vm-portability-round-3
tags:
- ESX
- Interoperability
- Macintosh
- Virtualization
- VMware
title: VM Portability, Round 3
url: /2006/12/12/vm-portability-round-3/
wordpress_id: 383
---

Having first tested [VM portability][1] from [VMware Workstation](http://www.vmware.com/products/ws/) (on Windows) to VMware Fusion (on [Mac OS X](http://www.apple.com/macosx/)), then [portability from ESX Server to VMware Fusion][2], the next logical step was VMware Fusion to ESX Server. I ran into one surprise but, as I expected, the process worked without any major hiccups.

The first step was to get the virtual disk files from my [MacBook Pro](http://www.apple.com/macbookpro/) over to [ESX Server](http://www.vmware.com/products/vi/esx/). Last time, when I ran this process in reverse, I used the open source SFTP client [Cyberduck](http://cyberduck.ch/) to copy the VMDK files down from ESX Server to my laptop. That worked well, but took a bit longer than I would have liked. This time, I dropped into the OS X Terminal and used command-line SCP. The command looked something like this (typed from the same directory where the VMDK file resided):

    scp ontap.vmdk esx01.example.net:/vmimages/uploads

(Full disclosure: I actually used a wildcard glob to pick up all the VMDK files, but you get the general idea.) This copied the files much more quickly than with Cyberduck. I believe this is less a comparison of Cyberduck vs. command-line SCP and more of a comparison between the SFTP and SCP protocols themselves. I intend to try the open source graphical SCP client [Fugu](http://rsug.itd.umich.edu/software/fugu/) to see if performance there supports that conclusion.

In any case, once the VMDK files were up on the host, it's `vmkfstools` once again to the rescue (again, typed from the directory where the original VMDK files resided):

    vmkfstools -i ontap.vmdk /vmfs/volumes/isanvol0/ontap/ontap.vmdk

Now that the VMDK files were on the ESX Server and imported into a VMFS datastore in the right format, I had only to create a virtual machine with the right configuration and attach it to the virtual disk. I suppose I could have copied the VMX file over as well and then used `vmware-cmd` to register the virtual machine....maybe next time.

Here's where it got weird (this is the one surprise I encountered). I created a new virtual machine in VirtualCenter but when I went to attach it to an existing virtual disk, _none of the virtual disks would show up._ I simply could not see them in the VI Client. And not just the VMDK files I'd just imported, but _any_ of the VMDK files. I double-checked permissions---no problems there. I re-imported the files with `vmkfstools`---no difference. Finally, I created a blank disk for the new virtual machine, then edited the VMX file to point to the VMDK file I'd imported. Then I powered on the VM.

Other than some USB and networking-related errors on the first boot (which were easily corrected by editing the VMX file), the VM worked exactly as expected. Other than the weirdness with locating the VMDK files to attach to the VM, the whole process took only 10 or 15 minutes, and most of that was copying data up to the ESX Server.

As I pointed out in my earlier articles on VM portability, this is proof-positive that VMware's virtualization layer provides an excellent vehicle for moving workloads across hardware as well as operating systems. Although I haven't included VMware Workstation on Linux in these tests, I fully expect it would behave in the same fashion as VMware Workstation for Windows. VMware Server should also be very much like, if not identical, to VMware Workstation as well. This gives us a solution that lets us scale workloads from workstation-class hardware, to server-class hardware (but still hosted on a host OS), then to enterprise-class virtual infrastructure (on the bare metal). Good stuff!

[1]: {{< relref "2006-11-29-vm-portability.md" >}}
[2]: {{< relref "2006-12-06-vm-portability-again.md" >}}
