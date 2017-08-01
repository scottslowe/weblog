---
author: slowe
categories: Information
comments: true
date: 2008-10-23T22:04:55Z
slug: virtualization-short-take-20
tags:
- ESXi
- HyperV
- iSCSI
- Microsoft
- NetApp
- NFS
- SAN
- Virtualization
- VMware
- VMwareHA
- WAFL
- ZFS
title: 'Virtualization Short Take #20'
url: /2008/10/23/virtualization-short-take-20/
wordpress_id: 998
---

This week's Short Take is a collection of links and articles that I've seen over the last few weeks (or longer ago, in some cases!) that I thought others might find interesting or useful. Enjoy!

* Alessandro broke the news to the general public about some [anticipated new virtualization features](http://www.virtualization.info/2008/10/windows-server-2008-r2-to-introduce.html) that are expected to make their debut in Windows Server 2008 R2, expected sometime in 2010. Microsoft announced [live migration for Hyper-V][1] back at the beginning of September, so that part was already known. Now coming from Alessandro's article is the announcement that Microsoft is developing a cluster file system, similar to VMFS, called Cluster Shared Volumes (CSV). Personally, this wasn't a big surprise to me as a contact of mine leaked this to me a while ago. Hopefully this won't hit Sanbolic too hard, whose Melio FS and Kayo FS solutions were intended to fill this gap (as discussed [here][2] and [here][3]).

* As fully expected, VMware and Microsoft trade lots of barbs back and forth about VMware ESX vs. Hyper-V and vice versa. Out of the various exchanges, I found the "Too Dry and Crunchy" exchange---now quite old, having been published back at the end of September---the most entertaining. It started [here with a barb from VMware](http://blogs.vmware.com/virtualreality/2008/09/esxi-vs-hyper-v.html) about how Hyper-V with Server Core, the recommended configuration from Microsoft for virtualization hosts, is "not the Windows you know." They compared Hyper-V on Server Core to ESXi and, not surprisingly, found ESXi to be easier and faster to install. What was really surprising though, was the response from James O'Neill in which he essentially agreed: Server Core _isn't_ "the Windows you know." While he does love Server Core, James also recognizes that Server Core is not the right fit for every workload, and that management processes and procedures may need to change when using Server Core. Personally, I'm glad to see James recognizing and being honest about the limitations (or caveats) of Server Core. If only all vendors were so honest about their own products...one day, perhaps.

* Duncan [points out](http://www.yellow-bricks.com/2008/10/14/virtualcenter-memory-statistic-definitions/) a great PDF on the definitions of various memory statistics. Readers may find that useful in understanding the various counters within VirtualCenter.

* This [VMware KB article](http://kb.vmware.com/selfservice/microsites/search.do?cmd=displayKC&docType=kc&externalId=1005476&sliceId=1&docTypeID=DT_KB_1_1&dialogID=36340738&stateId=0%200%202961456) outlines a potential VMware HA problem with multiple Service Console interfaces.

* Andy Leonard [picked up](http://andyleonard.com/2008/10/17/esx-swap-on-nfs-or-not/) this [VMware KB article](http://kb.vmware.com/selfservice/microsites/search.do?cmd=displayKC&docType=kc&externalId=1004082&sliceId=1&docTypeID=DT_KB_1_1&dialogID=2709533&stateId=0%200%202711273) that I bookmarked via Delicious.com and discussed how VMware's recommendations and NetApp's recommendations seem to run counter to each other. Personally, I'm inclined to follow VMware's recommendations after the little snafu with [NetApp's NFS file locking suggestion][4].

* This is a cool article on [the use of ZFS and iSCSI](http://blogs.sun.com/rarneson/entry/zfs_clones_iscsi_and_vmware) to create clones in storage instead of at the virtualization layer. This is interesting because it's being done with Solaris and ZFS, but it's functionally equivalent to FlexClones with NetApp, which I've discussed before (see [here][5], [here][6], and [here][7]). Accordingly, ZFS clones will suffer from all the same limitations as NetApp FlexClones.

* And while we're on the topic of Sun and NetApp, what's the deal with the recent patent rulings in the ZFS vs. WAFL lawsuit? If I'm reading [this update](http://blogs.sun.com/dillon/entry/one_more_thing) correctly, it looks like some of the core WAFL patents from NetApp are being invalidated. Is Sun going to win this thing?

That does it for now. Thanks for reading!

[1]: {{< relref "2008-09-08-live-migration-with-hyper-v.md" >}}
[2]: {{< relref "2008-06-16-melio-fs-hyper-v-and-vmware-esx.md" >}} 
[3]: {{< relref "2008-07-08-sanbolic-looking-to-capitalize-on-hyper-v-opportunity.md" >}}
[4]: {{< relref "2008-10-18-important-note-regarding-vmware-over-nfs.md" >}}
[5]: {{< relref "2007-05-11-how-to-provision-vms-using-netapp-flexclones.md" >}}
[6]: {{< relref "2007-05-15-netapp-flexclones-with-vmware-part-1.md" >}}
[7]: {{< relref "2007-05-17-netapp-flexclones-with-vmware-part-2.md" >}}
