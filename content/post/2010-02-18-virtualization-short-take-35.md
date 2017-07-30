---
author: slowe
categories: Information
comments: true
date: 2010-02-18T11:38:33Z
slug: virtualization-short-take-35
tags:
- HyperV
- Microsoft
- UCS
- Virtualization
- VMware
- vSphere
title: 'Virtualization Short Take #35'
url: /2010/02/18/virtualization-short-take-35/
wordpress_id: 1840
---

It's that time again: time for another Virtualization Short Take! Here's a collection of links, articles, posts, and other tidbits that I've found interesting, informative, or useful over the last few weeks. I hope that you find something useful as well!

* Tom Howarth has been spending some time with Microsoft's App-V application virtualization solution; he's written a three-part series ([Part 1](http://planetvm.net/blog/?p=1117), [Part 2](http://planetvm.net/blog/?p=1201), and [Part 3](http://planetvm.net/blog/?p=1223)). Part 1 discusses domain and certificate setup, Part 2 centers around policies and GPO settings, and Part 3 covers the client-side setup for App-V. While Tom's overview is extremely helpful, I don't recall seeing any thoughts on App-V as a product. Tom, did you like it? Not like it? What was good or bad about it? It would be great to have a post that brings this sort of information together.

* Interested in getting a better feel for the communications that occur between an ESX/ESXi host and vCenter Server? This post discusses [decoding SSL traffic with Wireshark](http://breathalize.co.uk/2010/01/26/decoding-ssl-traffic-between-a-vcentre-server-and-esx-host/) so that you can see what's happening.

* Jeremy Waldrop of Varrow has a good "getting started" post on [using vCenter Server's storage alarms](http://jeremywaldrop.wordpress.com/2010/01/24/vmware-vsphere-vcenter-storage-alarms/). If you're looking for an introductory piece, this is a good place to start.

* If you're using Hyper-V and have VMs that are generating lots of network traffic, this post from the Windows Server Performance Team discussing [increasing the VMBus buffer size](http://blogs.technet.com/winserverperformance/archive/2010/02/02/increase-vmbus-buffer-sizes-to-increase-network-throughput-to-guest-vms.aspx) is probably worth a look for you.

* And while I'm mentioning Hyper-V, Ben Armstrong aka Virtual PC Guy discusses an [RDP ActiveX control](http://blogs.msdn.com/virtual_pc_guy/archive/2010/02/03/hyper-v-activex-rdp-control.aspx) that provides RDP connectivity to a VM (not RDP connectivity to the guest OS, which is distinct and separate). I've never been a huge fan of ActiveX controls, but this could be useful in certain environments.

* Is defragmentation of VMs a good thing? Scott Drummonds asks the same question in [this blog post](http://vpivot.com/2010/02/12/windows-guest-defragmentation/). My only comment: avoid defragmentation with thin provisioned disks (array-level or hypervisor-level thin provisioning).

* Of course, Scott Drummonds also had a flurry of very useful posts over the last few weeks: [missing Perfmon counters](http://vpivot.com/2010/01/26/vmware-perfmon-counters-missing-on-vsphere/), [inaccuracy of guest performance counters](http://vpivot.com/2010/02/10/inaccuracy-of-in-guest-performance-counters/), and [Las Vegas taxi rates](http://vpivot.com/2010/02/11/las-vegas-taxi-rates/). (The Las Vegas taxi post actually helped me save some money when headed to the airport after PEX. Your mileage may vary---pun intended.)

* Eric Sloof's home-grown tests of [running linked clones on an SSD](http://www.ntpro.nl/blog/archives/1413-Hosting-20-linked-clones-on-SSD-storage.html) aren't definitive, but they definitely back up the value that has been seen with the deployment of EFDs (Enterprise Flash Drives) in virtualized environments.

* [This PowerShell script](http://virtualisedreality.com/2010/01/24/powershell-scipt-for-vmware-view-vsphere-who-is-logged-into-which-vm/) will show you the logged-in user for a given VMware View desktop. Handy!

* Readers seeking more information on guest OS alignment should read [this article by Jeff Muir](http://citrixblogger.org/2010/02/07/vhd-versus-ntfs-alignment/). While the focus of the article is on VHD and NTFS alignment, the underlying principles are also applicable to VMDK files in VMware environments.

* Frank Denneman, VCDX 29, has had a few good posts recently. He had a post that discusses [the use of local storage for VM swap](http://frankdenneman.nl/2010/02/impact-of-host-local-vm-swap-on-ha-and-drs/); this post was then parlayed into a greater discussion on understanding the impact of design decisions. It's a pretty fitting discussion given the timing around all the VCDX defense panels at Partner Exchange and Frank's own elevation to the VCDX priesthood. Frank's article on [VM sizing and NUMA](http://frankdenneman.nl/2010/02/sizing-vms-and-numa-nodes/) was also a great read. Keep up the good work, Frank! (And I'm still waiting to see all the info about memory reservations you promised me...)

* Jason Boche recently highlighted his adventures in [using Round Robin multipathing](http://www.boche.net/blog/index.php/2010/02/04/configure-vmware-esxi-round-robin-on-emc-storage/) with his EMC Celerra. One key takeaway is that he had to reboot the ESX/ESXi host after changing the SATP, so keep that in mind. There is also a very specific CLARiiON configuration that needs to be set: the Failover Mode needs to be set to 4.

* Jonathan Medd provides some great information on users who might be new to vCenter Update Manager in [this article](http://www.simple-talk.com/sysadmin/virtualization/using-vmware-vcenter-update-manager-to-keep-your-vsphere-hosts-up-to-date-with-patching/).

* If you are planning on virtualizing any SQL Server systems, be sure to check out this list of [best practices for SQL Server](http://communities.vmware.com/docs/DOC-8964), written by Scott Drummonds. The document is a bit old (December 2008), but the recommendations are still valid.

* It appears that VMware has updated [this KB article](http://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&externalId=1017652) recommending the use of the LSI Logic vSCSI controller for low I/O environments. I'm glad to see VMware has added more information and clarification; the previous version of the article was a bit spartan, to say the least.

* I think that Figure 1 on this page on [Cisco solutions for VMware View environments](http://www.cisco.com/en/US/docs/solutions/Enterprise/Data_Center/vmware/cisco_VMwareView.html) would give even [Hany Michael](http://www.hypervizor.com/) a run for the money! While Figure 1 is pretty complex, the information in the article is useful and helps underscore some of the many different ways Cisco products can be put to use in a VMware View environment.

* Here's a useful document on [integrating Cisco UCS with VMware DPM](https://supportforums.cisco.com/docs/DOC-8582).

* This [weekly summary of new KB articles](http://blogs.vmware.com/kbdigest/2010/02/new-articles-published-for-week-ending-02142010.html) is quite useful. OK, I know this isn't new and many people probably already knew about it but it's still useful. So get off my case, OK?

There's more that I could include, but I should probably wrap this up. Here are a few other links worth mentioning:

[The Backup Blog: Avamar and VMware Backup Revisited](http://thebackupblog.typepad.com/thebackupblog/2010/01/avamar-and-vmware-backup-revisited.html)  

[VMware KB: ESX 4.0 and ESXi 4.0 shutdown and reboot commands](http://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&externalId=1013193)  

[VMware KB: Masking a LUN from ESX and ESXi 4.0 using the MASK_PATH plug-in](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1009449)  

[Rethinking vNetwork Security](http://www.virtualizationpractice.com/blog/?p=4284)
[Announcing NVSPBind](http://blogs.technet.com/jhoward/archive/2010/01/25/announcing-nvspbind.aspx)

That's it for this time around. Thanks for reading and feel free to submit any interesting links you've found in the comments!
