---
author: slowe
categories: Information
comments: true
date: 2010-05-18T11:00:00Z
slug: virtualization-short-take-40
tags:
- Cisco
- ESXi
- Nexus
- Storage
- UCS
- Virtualization
- VMware
- vSphere
title: 'Virtualization Short Take #40'
url: /2010/05/18/virtualization-short-take-40/
wordpress_id: 1948
---

Welcome to the 40th post in the Virtualization Short Take series, where I share with you various virtualization-related links, thoughts, and news tidbits. (Occasionally, I throw in some stuff that's not virtualization related just to see if you are paying attention.) Enjoy!

* There have been a couple of posts now discussing Storage IO Control, a new feature that is possibly slated for inclusion in a future release of VMware vSphere. Storage IO Control extends the disk shares model cluster-wide, allowing administrators to properly shape access to back-end storage resources. The inimitable Scott Drummonds [discussed it](http://vpivot.com/2010/05/04/storage-io-control/) on Pivot Point (his blog), and Craig Stewart also recently published [an article about Storage IO Control](http://gestaltit.com/all/tech/storage/craig/storage-io-control-sioc-vmware-drs/) over at Gestalt IT. There's a fair amount of duplication between the two articles (Craig based his article partly on Scott's), but both are worth a read if you need to come up to speed on this new feature. _(Quick disclaimer: I'm discussing Storage IO Control here only because it's been mentioned elsewhere by others. As to whether or not this feature will or will not appear in a future release, or when that future release might be, I know nothing. OK?)_

* I don't know why, but I saw [this virtual appliance](http://www.vmware.com/appliances/directory/384) on VMware's web site earlier today and it has triggered a nagging feeling in the back of my head. Is the future of computing found in simple "scale out" building blocks like this?

* This [article on "VM stall"](http://pleasediscuss.com/andimann/20100514/is-%e2%80%98vm-stall%e2%80%99-the-next-big-virtualization-challenge/) by Andi Mann just re-confirms something I've been saying for a while: there are still too many companies out there that aren't taking full advantage of virtualization. If you're one of the almost 54% of companies that is still less than 30% virtualized, what's holding **you** back?

* Among all the other announcements from EMC World last week, this little tidbit might have gotten overlooked. Chad [blogged](http://virtualgeek.typepad.com/virtual_geek/2010/05/iscsi-clariion-and-vsphere-nice-fix.html) about a fix contained in FLARE 30--the updated version announced at the conference---that addresses a problem with iSCSI initiators and the CLARiiON arrays. Good work, EMC engineering!

* Over on VMware's ESXi Chronicles blog, Charu Chaubal recently published a two-part series on hardware monitoring via CIM ([part 1](http://blogs.vmware.com/esxi/2010/04/hardware-health-monitoring-via-cim.html) and [part 2](http://blogs.vmware.com/esxi/2010/05/hardware-health-monitoring-via-cim-part-2.html)).

* A reader dropped me an e-mail about an issue uncovered in their environment while trying to automate the VMware Tools installation. Apparently, the VMware Tools installation relies on 8.3 file naming conventions. Normally, this wouldn't be a problem, but in environments where 8.3 file name creation is disabled...well, you can see where there might be a problem. No workaround has yet been found. Any wizards out there who have suggestions are welcome to add them to the comments of this post.

* Two posts popped up in the last couple of weeks regarding the default number of ports on a Nexus 1000V port profile: [this post by Kevin Goodman](http://blog.colovirt.com/2010/05/05/cisco-networking-vmware-cisco-nexus-1000v-increasing-default-max-ports/) and [this post by Jason Nash](http://jasonnash.wordpress.com/2009/10/23/out-of-ports-on-a-nexus-1000v-port-profile/). Fortunately, it's a quick process to increase the default maximum of 32 ports.

* Running your VMware vSphere environment on NFS? Have a look at [this document](http://www.vmware.com/resources/techresources/10096) from VMware.

* Didier Pironet of DeinosCloud, the same gentleman who showed us how to increase the number of VMware HA primary nodes, posted a guide on [adjusting the memory usage of Tomcat](http://deinoscloud.wordpress.com/2009/11/30/tomcat-for-vcenter-memory-tuning/) (the engine behind VMware vCenter's web services). This is most likely an unsupported configuration change, but it might be handy in test/development environments.

* Kevin Goodman also had a good post on [configuring EMC PowerPath on Linux on Cisco UCS](http://blog.colovirt.com/2010/05/04/storage-san-linux-emc-powerpath-configuration-on-cisco-ucs/). I know, I know: this isn't strictly related to virtualization, but it's close enough in my book.

* Alastair of demitasse.co.nz finally declares that the emperor has no clothes when he states why he believes [users don't want a client hypervisor](http://www.demitasse.co.nz/wordpress2/2010/05/why-users-dont-want-a-client-hypervisor-warning-opinion-inside/). Personally, I tend to agree with him; I think a hosted hypervisor is far more valuable on the client-side space (especially in the BYOPC scenario). Just because you _can_ run a bare metal hypervisor on your laptop doesn't necessarily mean that you _should_ run a bare metal hypervisor on your laptop.

* There seems to be a fair amount of confusion around the vStorage APIs; perhaps this is due to the different subsets of the vStorage APIs. There are the vStorage APIs for Array Integration (VAAI); these were discussed in some detail last week at EMC World. There are also the vStorage APIs for Multipathing (VAMP), which serve to support multipathing plugins like PowerPath/VE. Finally, there are the vStorage APIs for Data Protection (VADP), which are the APIs that serve to replace VMware Consolidated Backup. If you'd like to know more about VADP in particular, [this VMware KB article](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1021175) has a list of frequently asked questions about VADP.

* Tom Howarth has brought to light [a potentially serious problem with Changed Block Tracking (CBT)](http://planetvm.net/blog/?p=1520), a key part of the vStorage APIs for Data Protection (VADP) that enables lots of backup and recovery applications.

* While reviewing one of the weekly VMware KB digests, I came across [this VMware KB article](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1020718) in which virtual NICs are sometimes detected as removable hardware; this can, in turn, cause the virtual NIC to disappear from the virtual machine. It appears that the only workaround for this behavior is to disable HotPlug.

* Xsigo recently posted [a comparison](http://www.xsigo.com/blog/?p=764) of their I/O virtualization solution vs. other I/O virtualization solutions. They include FCoE as an I/O virtualization solution, but as [I've said in the past][1] I don't consider FCoE an I/O virtualization solution. To include FCoE in this sort of comparison is kind of like saying that an apple is a poor orange because it lacks a thick outer skin. FCoE wasn't _designed_ to do I/O virtualization---it was _designed_ to carry Fibre Channel traffic over Ethernet. Despite the liberty with which comparative technologies are selected, the article is worth reading nevertheless.

And to round out this issue of Virtualization Short Takes, here are a few "bonus links" I found:

[UCS with disjointed L2 domains](http://www.unifiedcomputingblog.com/?p=132)  

[The "Mini-Rack" Approach to Blade Server Design](http://www.mseanmcgee.com/2010/05/the-mini-rack-approach-to-blade-server-design/)  

[Hot adding or removing a Cisco 3750 from a stack](http://www.vmguru.nl/wordpress/2010/03/hot-adding-or-removing-a-cisco-3750-from-a-stack/)  

[EMC World Cubed - Here's all the Video](http://blogstu.wordpress.com/2010/05/14/emc-world-cubed/)  

[ESXTOP, My understanding](http://geeksilver.wordpress.com/2010/05/17/esxtop-my-understanding/)

That's it for now. I hope you found something useful. Feel free to share more useful links in the comments, and thanks for reading!

[1]: {{< relref "2008-11-17-fcoe-versus-mr-iovhuh.md" >}}
