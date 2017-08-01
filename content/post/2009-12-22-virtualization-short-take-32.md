---
author: slowe
categories: Information
comments: true
date: 2009-12-22T23:14:44Z
slug: virtualization-short-take-32
tags:
- Networking
- Storage
- Virtualization
- VMware
- vSphere
title: 'Virtualization Short Take #32'
url: /2009/12/22/virtualization-short-take-32/
wordpress_id: 1772
---

Here it is---the last Virtualization Short Take of 2009! This is just a small collection of various virtualization-related links and articles that I've gathered over the last few weeks (OK, maybe more than a few weeks).

* Scott Drummonds posted a good article with a [performance comparison](http://vpivot.com/2009/11/18/performance-of-thin-provisioned-disks/) of thick provisioned disks vs. thin provisioned disks. This is good information and helps to clear up a lot of misinformation about the behavior of thin provisioned disks.

* Interested in using the software iSCSI initiator with a dvSwitch and multiple paths? [This article](http://goingvirtual.wordpress.com/2009/12/01/vsphere-4-0-update-1-with-software-iscsi-and-2-paths-on-dvswitch/) can give you the information you need to get started.

* Frank Denneman has a great article providing some [details on memory reservations and memory usage](http://frankdenneman.wordpress.com/2009/12/08/impact-of-memory-reservation/). It's definitely worth a read, as is this post on the [impact of guest OS type mismatch](http://frankdenneman.wordpress.com/2009/12/15/impact-of-mismatch-guest-os-type/). See, there is a reason why it's important to make sure you select the _correct_ guest OS type when creating VMs.

* I've written about the paravirtualized SCSI (PVSCSI) driver [before][1], but Scott Sauer over at Virtual Insanity has a great two-part series on PVSCSI that is definitely worth a read ([part 1](http://www.virtualinsanity.com/index.php/2009/11/21/more-bang-for-your-buck-with-pvscsi-part-1/) and [part 2](http://www.virtualinsanity.com/index.php/2009/12/01/more-bang-for-your-buck-with-pvscsi-part-2/)).

* Although a bit dated now, Dave Lawrence has a great [review of some of November's technical white papers](http://vmguy.com/wordpress/index.php/archives/1248). He references the thin provisioning performance paper that's also referenced in Scott Drummond's post above, as well as a white paper on optimizing SRM performance. Thanks for bringing our attention to these documents, Dave!

* The release of VMware View 4 brings with it PCoIP, which is supposed to bring enhanced performance over older display protocols. Unfortunately, everything I was hearing was that PCoIP was incompatible with many WAN acceleration solutions. So I was quite puzzled at [this press release](http://www.businesswire.com/portal/site/home/permalink/?ndmViewId=news_view&newsId=20091207005609&newsLang=en). Reading the press release a bit more closely, though, it would seem that Expand's solution doesn't actually optimize or accelerate PCoIP; rather, it enables it to be tunneled and applies Quality of Service (QoS). Something is better than nothing, I guess.

* Rick Scherer [describes a strange issue](http://vmwaretips.com/wp/2009/12/09/strange-vcenter-40-u1-and-esxi-40-u1-ssl-issue/) with vCenter Server 4.0 Update 1 and VMware ESXi 4.0 Update 1. Rick was seeing a number of strange symptoms, including ESXi hosts suddenly disconnecting from vCenter Server. Last time I checked Rick still hadn't identified the root cause, although the symptoms he was seeing have since disappeared.

* Stu provides [a thorough explanation](http://vinternals.com/2009/12/why-you-shouldnt-update-vcenter-if-using-esxi-yet/) of why VMware is recommending not to install vCenter Server 4.0 Update 1 when managing VMware ESXi 4.0 hosts.

* Need to install the HP Management Agents on VMware ESX 4.0? Here are some [helpful instructions](http://blog.mrpol.nl/2009/11/03/installing-hp-insight-management-agents-on-vmware-vsphere-4-server/).

* Nigel Poulton has started a great series on rack area networks (RANs), of which a key component is I/O virtualization. So far, Nigel has published [part 1](http://blog.nigelpoulton.com/ran-rack-area-networking/) (introducing the concept of a RAN), [part 2](http://blog.nigelpoulton.com/rack-area-networking-iov/) (IOV's role in a RAN), and [part 3](http://blog.nigelpoulton.com/ran-iov-and-hairpin-turns/) (IOV and hairpin turns). Part 2, in particular, has a good discussion of SR-IOV and MR-IOV. Nigel's discussion of SR-IOV is a good complement to [my own][2].

* Greg Schulz also has [a lengthy article](http://storageio.com/blog/?p=729) on I/O virtualization.

* Curious to know why NUMA is important with vSphere? Network Computing blogger Jake McTigue (with whom I had the honor of participating on a recent virtual networking webcast) has a [good overview of NUMA](http://www.networkcomputing.com/virtualization/harnessing-vsphere-performance-benefits-for-numa.php) and what it means for virtualized environments. It's worth a read if you're not already familiar with NUMA.

* Need more information on storage alignment and VMFS block sizes? Check out [this VIOPS document](http://viops.vmware.com/home/docs/DOC-1407).

I also have a whole list of other links that I haven't had the chance to read yet but that look like they might be interesting or useful:

[A handy new addition to the Command Line Tool for View 4](http://www.virtualinsanity.com/index.php/2009/12/08/a-handy-new-addition-to-the-command-line-tool-for-view-4/)  

[Whats what in VMware View and VDI Land](http://virtualgeek.typepad.com/virtual_geek/2009/12/whats-what-in-vmware-view-and-vdi-land.html)  

[How to get PCoIP with View 4 to work every time!](http://www.thatsmyview.net/2009/12/18/how-to-get-pcoip-with-view-4-to-work-every-time/)  

[Revisiting the Components of the Cisco Nexus 1000v](http://jasonnash.wordpress.com/2009/12/18/revisiting-the-components-of-the-cisco-nexus-1000v/)  

[File Virtualization The short primer](http://devcentral.f5.com/weblogs/dmacvittie/archive/2009/12/06/file-virtualizationhellip-the-short-primer.aspx)  

[Is Your Blade Ready for Virtualization? A Math Lesson](http://www.dailyhypervisor.com/2009/12/19/is-your-blade-ready-for-virtualization-a-math-lesson/comment-page-1/#comment-213)  

[RSA SecureBook for VMware View hardening now publicly available!](http://virtualgeek.typepad.com/virtual_geek/2009/12/rsa-securebook-for-vmware-view-hardening-now-publicly-available.html)

I guess that's enough for now. If you have any other useful, unique, or interesting virtualization-related links, feel free to share them in the comments. Thanks for reading!

[1]: {{< relref "2009-07-05-another-reason-not-to-use-pvscsi-or-vmxnet3.md" >}}
[2]: {{< relref "2009-12-02-what-is-sr-iov.md" >}}
