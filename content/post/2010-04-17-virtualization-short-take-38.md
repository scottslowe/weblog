---
author: slowe
categories: Information
comments: true
date: 2010-04-17T12:06:54Z
slug: virtualization-short-take-38
tags:
- Storage
- Virtualization
- VMware
- vSphere
title: 'Virtualization Short Take #38'
url: /2010/04/17/virtualization-short-take-38/
wordpress_id: 1882
---

I really should get into the habit of posting these short takes more often! It's been a while since the last Virtualization Short Take and now I have a huge collection of items that I wanted to mention. Anyway, without further ado, here's Virtualization Short Take #38!

* If you are looking for some additional information on creating unattended installation scripts for VMware ESX 4.0, be sure to check out [this post](http://ict-freak.nl/2010/03/23/vsphere-unattended-esx4-installation-with-a-ks-cfg-file/) from Arne Fokkema. Arne's post on [hot adding or removing a VMDK with a Linux VM](http://ict-freak.nl/2010/03/30/vsphere-hot-add-or-remove-a-vmdk-with-a-linux-vm/) is worth a read, too.

* Similarly, those of you interested in running ESXi on USB should be sure to check out Simon Seagrave's article on [the "official" way](http://www.techhead.co.uk/installing-vmware-esxi-4-0-on-a-usb-memory-stick-the-official-way) to install VMware ESXi 4.0 to a USB stick.

* Maish has a couple of good posts that I picked up recently. First, he provides some step-by-step instructions on [injecting VMware drivers into the source OS](http://technodrone.blogspot.com/2010/03/inject-vmware-drivers-into-source-os.html) before performing a physical-to-virtual (P2V) conversion (here is [the VMware KB article](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1005208) upon which Maish's article is based). Next up is a good article on [installing PowerPath/VE using VMware Update Manager](http://technodrone.blogspot.com/2010/03/install-emc-powerpathve-with-update.html) (again, here's the source [VMware KB article](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1018740)).

* For the ThinApp lovers out there (and what's not to love?), [this site](http://www.thindownload.com/) has ThinApped versions of many open source software packages.

* As is usually the case, Duncan Epping has a number of very informative articles. First, he [lifts the veil](http://www.yellow-bricks.com/2010/03/29/cool-new-ha-feature-coming-up-to-prevent-a-split-brain-situation/) on an upcoming feature to assist with VMware HA split brain scenarios. Next up is a great discussion about the virtue of [reducing the I/O operations limit](http://www.yellow-bricks.com/2010/03/30/whats-the-point-of-setting-iops1/) (for Round Robin multipathing using vSphere native multipathing) down to 1 ([more information on this topic](http://virtualgeek.typepad.com/virtual_geek/2010/03/understanding-more-about-nmp-rr-and-iooperationslimit1.html) from Chad's blog as well). Equally valuable as the article are the comments on the article! Finally Duncan also tackles the concept of [VM disk alignment](http://www.yellow-bricks.com/2010/04/08/aligning-your-vms-virtual-harddisks/), which is not a new issue but it's great to see it getting more attention. Keep up the great work Duncan!

* Didier Pironet of DeinosCloud has information on [how to increase the number of VMware HA primary nodes](http://deinoscloud.wordpress.com/2010/03/26/increase-number-of-vmware-ha-primaries-how-to/) beyond 5. It's **very important** to note that this is an unsupported configuration, but the article does provide a bit of insight on the configuration of VMware HA if nothing else.

* Gabe brings to light [a potential problem](http://www.gabesvirtualworld.com/bug-in-vmware-ha-admission-control-when-using-dpm/) with the interaction between VMware DPM and VMware HA admission control (there's also a [VMware KB article](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1007006) about it). If you run VMware DPM and VMware HA with admission control disabled, be sure to have a look at these two resources for more information.

* I also came across [this article](http://www.alexfeigenson.com/2010/04/hp-esx-management-agents-wont-uninstall/) which describes some undocumented command-line paramters for the `esxupdate` command. Pretty handy!

* Jason Boche describes [a bug in the ESX(i) failback behavior](http://www.boche.net/blog/index.php/2010/04/07/no-failback-bug-in-esxi/) (or, more appropriately, the no failback behavior) that could have some pretty serious side effects if you're not careful with your design. Jason also has [a good post](http://www.boche.net/blog/index.php/2010/03/28/windows-2008-r2-and-windows-7-on-vsphere/) on the manual steps required in order to use the WDDM video driver for Windows 7 and Windows Server 2008 R2.

* As [pointed out here](http://vinf.net/2010/04/05/installing-vcenter-on-windows-2008-r2-x64-there-is-no-dsn-which-can-be-used/), don't forget to install the SQL Native Client if you're running Windows Server 2008 R2 x64 for your vCenter Server; otherwise, you'll find yourself unable to create a compatible DSN. Note that I think this applies to a few other versions of Windows as well; I know that I've had to do it a few times.

* [This post](http://bitpushr.wordpress.com/2010/04/02/ballooning-memory-shared-storage-file-io-performance/) underscores the need to understand the relationships between all the various components of a VMware vSphere deployment and to understand the impact of various design decisions. The post outlines an issue where IP-based storage is used and memory pressures are forcing storage connectivity issues because of increased utilization of VM swap (which is also stored on the IP storage array). There are also follow up posts [here](http://bitpushr.wordpress.com/2010/04/02/ballooning-results-are-in/) and [here](http://bitpushr.wordpress.com/2010/04/03/conclusion-and-possible-vindication/) with more information and results from some testing.

* Microsoft recently released MED-V 1.0 SP1 and localized versions of App-V 4.6, as announced [here](http://blogs.technet.com/mdop/archive/2010/04/01/med-v-1-0-sp1-rtm-and-localized-versions-of-app-v-4-6-are-now-available.aspx).

* In reviewing the weekly KB digests, I found a few interesting articles:  

[Changing Parvirtualized SCSI Controller Queue Depth](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1017423)  

[Guide to configure NTP on ESX servers](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1003063)  

[Testing a VMware Fault Tolerance Configuration](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1020058)  

[High rate of Fault Tolerance failovers on Intel Xeon Processor 3400, 6500, and 7500 series](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1017520)

* Via [this post](http://frankdenneman.nl/2010/03/identify-storage-performance-issues/) by Frank Denneman, I learned of [VMware KB article 1008205](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1008205) on how to use `esxtop` to identify storage performance issues. Also be sure to check out a few of the other links in Frank's article, all of which also provide good information.

* VMware's competitors love to harp on memory overcommitment, which can---if used improperly and inappropriately---have a negative impact on performance. As a result, I was glad to see this post by Scott Drummonds on [using memory reservations to drive overcommitment](http://vpivot.com/2010/03/31/memory-reservations-drive-over-commit/) for optimal utilization of memory on your ESX/ESXi hosts. Good post, Scott!

OK, that does it for this time around. As always, feel free to speak up in the comments if you have additional information, links, clarifications, or suggestions.
