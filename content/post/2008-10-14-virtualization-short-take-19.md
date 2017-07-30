---
author: slowe
categories: Information
comments: true
date: 2008-10-14T21:53:11Z
slug: virtualization-short-take-19
tags:
- HyperV
- NFS
- VDI
- Virtualization
- VMFS
- VMware
- VMwareHA
title: 'Virtualization Short Take #19'
url: /2008/10/14/virtualization-short-take-19/
wordpress_id: 987
---

Virtualization Short Take #19 is here, with news, headlines, and commentary on a few things that have passed my way over the last few weeks. Feel free to share your own interesting tidbits in the comments!

* Rick Blythe, aka VMwarewolf, regales us with a tale of a new "feature" in VirtualCenter 2.5 Update 2. This new feature [prevents VM migrations](http://www.vmwarewolf.com/host-wont-go-into-maintenance-mode/) when putting a VMware ESX server into maintenance mode if it will violate the VMware HA failover level. Users will need to disable VMware HA in order to restore the pre-Update 2 functionality. (Anyone know if this has been addressed in VirtualCenter 2.5 Update 3?)

* This [VMware KB article](http://kb.vmware.com/selfservice/microsites/search.do?cmd=displayKC&docType=kc&externalId=1004082&sliceId=1&docTypeID=DT_KB_1_1&dialogID=2709533&stateId=0%200%202711273) is quite interesting; note the excerpt under the "Purpose" section of the article:  

	>VMware recommends storing your swap on a VMFS3 volume, when running virtual machines on NFS storage.

	So, even when running your VMs on NFS, VMware still recommends running the VM swap files on a VMFS3 volume. Very interesting, indeed. This is particularly interesting to NetApp, who---some would say rightly so---heavily pushes NFS for VMware storage.

* Also from VMwarewolf, a note about [guest customization failing](http://www.vmwarewolf.com/guest-customization-fails-on-virtualcenter-25-update-2/) with VirtualCenter 2.5 Update 2. Again, anyone know if this has been addressed with Update 3?

* Microsoft has released a hotfix for Hyper-V failover clustering that improves functionality and adds VM control. An [article at Hypervoria](http://hypervoria.com/hyper-v/hyper-v-failover-cluster-hotfix-now-public.aspx) initially alerted me to this hotfix; a [full Microsoft KB article](http://support.microsoft.com/kb/951308/en-us) is also available.

* Ben Armstrong aka Virtual PC Guy provides a couple of scripts---in VBScript and PowerShell---for [creating a dynamic VHD](http://blogs.msdn.com/virtual_pc_guy/archive/2008/09/25/hyper-v-scripting-dynamic-vhd-creation.aspx) file.

* Again [via Ben](http://blogs.msdn.com/virtual_pc_guy/archive/2008/09/24/symantec-backup-exec-for-windows-available-with-hyper-v-support.aspx), it looks as if Symantec has added [support for Hyper-V to Backup Exec 12.5](http://www.symantec.com/business/products/newfeatures.jsp?pcid=pcat_storage&pvid=57_1). That's interesting, because I asked about Hyper-V support back in June at Tech-Ed and was told "as market demands dictate". Is market demand now dictating?

* Duncan alerts us to a potential [failure of Storage VMotion](http://www.yellow-bricks.com/2008/09/29/storage-vmotion-fails-after-service-console-ip-change/) after changing the Service Console IP address. I personally haven't seen this behavior, but the fix is handy to know just in case.

* Looking for a way to make mass changes to some VMX files? Luckily for you, the VI Toolkit blog has [some information](http://blogs.vmware.com/vipowershell/2008/09/changing-vmx-fi.html) that you might find useful.

* Brian Madden has a [good overview of "View Composer"](http://www.brianmadden.com/blog/BrianMadden/A-deeper-look-at-VMwares-upcoming-View-Composer-VDI-disk-image-technology-ie-multiple-VMs-sharing-the-same-disk-image) as it was described a few weeks ago at VMworld. Toward the end of the article, Brian mentions that VMware hasn't announced anything with regard to user profiles:  

	>While that sounds noble, it also is a bit at odds with the longer-term vision that VMware CEO Paul Maritz outlined in the VMworld keynote, namely, that VMware wants to focus on deploying a personality to a user, not to a device. Certainly View Composer goes a long way in centrally managing desktops, but I wouldn't be surprised if VMware does more in the user personalization space in the future as well.

	As Warren Ponder points out in the comments, there are several ways to handle user profiles, and it does sound like VMware already has some irons in the fire to help address that particular concern.

* This has been pointed out in numerous places around the web, but who can fault one more link? Mike DiPetrillo of VMware has re-created Hyper-V's [Quick Migration functionality using PowerShell](http://mikedatl.typepad.com/mikedvirtualization/2008/10/quick-migration.html). I could go somewhere with this, but I think I'll just leave it alone.

I guess that will do it for this time around. Again, feel free to share links or tidbits you found interesting in the comments below.
