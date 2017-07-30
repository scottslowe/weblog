---
author: slowe
categories: Tutorial
comments: true
date: 2009-06-01T10:49:57Z
slug: vsphere-virtual-machine-upgrade-process
tags:
- Microsoft
- Virtualization
- VMware
- vSphere
- Windows
title: vSphere Virtual Machine Upgrade Process
url: /2009/06/01/vsphere-virtual-machine-upgrade-process/
wordpress_id: 1382
---

Upgrading a VMware Infrastructure 3.x environment to VMware vSphere 4 involves more than just upgrading vCenter Server and upgrading your ESX/ESXi hosts (as if that wasn't enough). You should also plan on upgrading your virtual machines. VMware vSphere introduces a new hardware version (version 7), and vSphere also introduces a new paravirtualized network driver (VMXNET3) as well as a new paravirtualized SCSI driver (PVSCSI). To take advantage of these new drivers as well as other new features, you'll need to upgrade your virtual machines. This process I describe below works really well.

I'd like to thank [Erik Bussink](http://bussink.ch/erik/), whose [posts on Twitter](http://twitter.com/erikbussink) got me started down this path.

Please note that this process **will require** some downtime. I personally tested this process with both Windows Server 2003 R2 as well as Windows Server 2008; it worked flawlessly with both versions of Windows. (I'll post a separate article on doing something similar with other operating systems, if it's even possible.)

1. Record the current IP configuration of the guest operating system. You'll end up needing to recreate it.

2. Upgrade VMware Tools in the guest operating system. You can do this by right-clicking on the virtual machine and selecting Guest > Install/Upgrade VMware Tools. When prompted, choose to perform an automatic tools upgrade. When the VMware Tools upgrade is complete, the virtual machine will reboot.

3. After the guest operating system reboots and is back up again, shutdown the guest operating system. You can do this by right-clicking on the virtual machine and selecting Power > Shutdown Guest.

4. Upgrade the virtual machine hardware by right-clicking the virtual machine and selecting Upgrade Virtual Hardware.

5. In the virtual machine properties, add a new network adapter of the type VMXNET3 and attach it to the same port group/dvPort group as the first network adapter.

6. Remove the first/original network adapter.

7. Add a new virtual hard disk to the virtual machine. Be sure to attach it to SCSI node 1:_x_; this will add a second SCSI adapter to the virtual machine. The size of the virtual hard disk is irrelevant.

8. Change the type of the newly-added second SCSI adapter to VMware Paravirtual.

9. Click OK to commit the changes you've made to the virtual machine.

10. Power on the virtual machine. When the guest operating system is fully booted, log in and recreate the network configuration you recorded for the guest back in step 1. Windows may report an error that the network configuration is already used by a different adapter, but proceed anyway. Once you've finished, shut down the guest operating system again.

11. Edit the virtual machine to remove the second hard disk you just added.

12. While still in the virtual machine properties, change the type of the original SCSI controller to VMware Paravirtual (**NOTE:** See update below.)

13. Power on the virtual machine. When the guest operating system is fully booted up, log in.

14. Create a new system environment variable named DEVMGR_SHOW_NONPRESENT_DEVICES and set the value to 1.

15. Launch Device Manager and from the View menu select Show Hidden Devices.

16. Remove the drivers for the old network adapter and old SCSI adapter. Close Device Manager and you're done!

If you perform these steps on a template, then you can be assured that all future virtual machines cloned from this template also have the latest paravirtualized drivers installed for maximum performance.

Post any questions or clarifications in the comments. Thanks!

**UPDATE:** Per [this VMware KB article](http://kb.vmware.com/selfservice/microsites/search.do?cmd=displayKC&externalId=1010398), VMware doesn't support using the PVSCSI adapter for boot devices. That is not to say that it doesn't work (it does work), but that it is not _supported._ Thanks to Eddy for pointing that out in the comments!
