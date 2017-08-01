---
author: slowe
categories: Information
comments: true
date: 2008-12-15T11:53:29Z
slug: virtualization-short-take-24
tags:
- ActiveDirectory
- EMC
- ESX
- HyperV
- iSCSI
- NFS
- SSL
- VMotion
- VMware
- VMwareSRM
- Virtualization
title: 'Virtualization Short Take #24'
url: /2008/12/15/virtualization-short-take-24/
wordpress_id: 1095
---

There's lots of good information flowing around the Internet, and it's becoming increasingly difficult to sort through all the useless stuff to find the valuable gems. Hopefully, some of the links that I have collected here will prove to be more useful than useless!

* I came across [this VMware KB article](http://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&externalId=1002722&sliceId=2&docTypeID=DT_KB_1_1&dialogID=9988974&stateId=1%200%209994223) titled "Dedicating specific NICs to portgroups while maintaining NIC teaming and failover for the vSwitch". I was hoping it would shed new light on some NIC teaming functionality. Unfortunately, it was only about overriding the default vSwitch failover policy on a per-portgroup basis. I was already well aware of that functionality and use it quite extensively in my VMware designs, but for others that may prove useful.

* [This video](http://www.youtube.com/watch?v=7CbRS0GGuNc) about VMware DPM sparked some debate about spin-up/spin-down affecting drive MTBF and decreasing a VMware ESX server's operational lifecycle. Chad Sakac of EMC shared some findings from EMC regarding spin-up/spin-down in [this post](http://virtualgeek.typepad.com/virtual_geek/2008/12/does-vmware-dpm-shorten-esx-server-lifespan.html) and came to the conclusion that using VMware DPM should not materially affect the reliability or lifetime of servers (at least with regards to drive failures). Personally, I tend to agree that this was FUD, most likely from a competitor, but it's best to get this sort of thing out in the open and debunked.

* Leo posted [a brief snippet of code](http://blog.core-it.com.au/?p=351) to upgrade the VMware Tools on VMs without a reboot. It looks like it might come in handy. And Leo's guide to [configuring jumbo frames with an EMC AX4-5i](http://blog.core-it.com.au/?p=100) is quite useful, too---it's a nice counterpoint to my own [guide to configuring jumbo frames][1].

* Tomas ten Dam has [completed his guide](http://tendam.wordpress.com/2008/11/18/srm-in-a-box-final-release-the-complete-setup/) to building a complete "SRM in a Box" setup using the NetApp Data ONTAP Simulator. Of course, Chad wants him to use the Celerra VM...

* Oh, and while we're talking VMware SRM, be sure to check out [Mike Laverick's](http://www.rtfm-ed.co.uk/) book on VMware SRM, "Administering VMware Site Recovery Manager 1.0". I haven't read the book yet, but knowing Mike I'm sure it's good quality stuff. Maybe Santa will give me a copy for Christmas.

* Sven H. over at VirtualFuture.info posted [a good guide on using thin provisioned VMDKs](http://virtualfuture.info/2008/12/vmware-esx-35-and-thinprovisioning/) with VMware ESX 3.5 via the vmkfstools command. (I was going to include a trackback to Sven's post, but his blog theme doesn't show the trackback URL.) Seems like I saw somewhere that thin provisioned VMDKs in ESX 3.5 are still unsupported, so deploy accordingly.

* [Via Tony Soper](http://blogs.technet.com/tonyso/archive/2008/11/26/hyper-v-how-to-patch-vms-offline.aspx), I found that version 2 of Microsoft's Offline Virtual Machine Servicing Tool is available. I first discussed the Offline Virtual Machine Servicing Tool [back in June][2] during Tech-Ed 2008. You can download the tool [here](http://www.microsoft.com/downloads/details.aspx?FamilyId=8408ECF5-7AFE-47EC-A697-EB433027DF73&displaylang=en).

* Also from Tony, here's a great article on [how to balance VM I/O with Hyper-V](http://blogs.technet.com/tonyso/archive/2008/12/04/hyper-v-how-to-balance-vm-i-o.aspx). An interesting tidbit from this: by default, I/O balancing is enabled for storage, but not for networking. I can see it needing to be enabled for storage, but why disabled by default for networking?

* More information on [controlling resource utilization within Hyper-V](http://www.virtualizationadmin.com/articles-tutorials/microsoft-hyper-v-articles/general/controlling-processor-resources-hyper-v-guests.html) is provided in this article by Robert Larson. It's worth having a quick look if you are unsure how to configure it or how it works.

* Ben Armstrong [answers](http://blogs.msdn.com/virtual_pc_guy/archive/2008/12/10/why-does-it-take-so-long-to-create-a-fixed-size-virtual-hard-disk.aspx) the question, "Why does it take so long to create a fixed size virtual hard disk?" The answer: the disk space is zeroed out in advance. My question is this: is this need to zero out the disk space a result of how NTFS deletes files or is this scenario applicable to VMFS as well?

* This has probably been mentioned before, but users considering virtualizing their Active Directory domain controllers should keep [these considerations](http://support.microsoft.com/kb/888794) in mind.

* I recently ran into a situation where we need to change the IP address of an NFS datastore. (It's a long story as to how this came about.) In any case, I told the customer that I couldn't be sure that changing the IP address wouldn't cause problems. Fortunately, before the customer tried it, I found [this post](http://vmwaretips.com/wp/2008/09/13/changing-the-ip-of-your-nfs-datastore/) by Rick Scherer. The short story: it doesn't work, and you shouldn't do it. Create a new datastore with the correct IP address and use Storage VMotion instead.

* For even more information on Storage VMotion, also check out [Chad's post here](http://virtualgeek.typepad.com/virtual_geek/2008/12/real-world-experiences-using-storage-vmotion.html).

* VMwarewolf continues his Resolution Path series with [common fault issues in VMware Infrastructure](http://www.vmwarewolf.com/common-fault-issues-in-vmware-infrastructure/). Good stuff.

It's clearly been too long since I published one of these, as I still have other links collecting dust in my "link bin":

[Third Brigade offers free security for up to 100 virtual machines](http://www.virtualization.info/2008/12/third-brigade-offers-free-security-for.html)  

[Version 4 of the PowerVDI tool](http://virtualgeek.typepad.com/virtual_geek/2008/11/version-9-of-the-powervdi-tool.html)  

[Go Daddy Wildcard Certificate with VI3](http://www.jasemccarty.com/blog/2008/01/godaddy-wildcard-certificate-with-vi3.html)  

[New VMware VI network port diagram request for comments](http://www.boche.net/blog/?p=655)  

[Auditing ESX root logins with email...](http://blog.core-it.com.au/?p=367)

Like I said, there's just so much information! And now that I'm trying to delve deeper into the storage realm, that's only doubled up on the information I'm trying to manage. Hopefully I've picked out a few gems for you this week. Thanks for reading!

[1]: {{< relref "2008-04-22-esx-server-ip-storage-and-jumbo-frames.md" >}}
[2]: {{< relref "2008-06-11-mgt374-offline-virtual-machine-servicing-tool.md" >}}
