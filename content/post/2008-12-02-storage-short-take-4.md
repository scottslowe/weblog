---
author: slowe
categories: Information
comments: true
date: 2008-12-02T09:00:06Z
slug: storage-short-take-4
tags:
- Deduplication
- EMC
- NetApp
- NFS
- SAN
- Snapshots
- Solaris
- Storage
- Sun
- VMware
- ZFS
title: 'Storage Short Take #4'
url: /2008/12/02/storage-short-take-4/
wordpress_id: 1073
---

Last week I provided a list of virtualization-related items that had made their way into my Inbox in some form or another; today I'll share storage-related items with you in Storage Short Take #4! This post will also be cross-published to [the Storage Monkeys Blogs](http://blogs.storagemonkeys.com/).

* Stephen Foskett has a nice [round-up of some of the storage-related changes](http://blog.fosketts.net/2008/11/07/storage-vmware-esx-update-3/) available to users in VMware ESX 3.5 Update 3. Of particular note to many users is the VMDK Recovery Tool. Oh, and be sure to have a look at Stephen's list of [top 10 innovative enterprise storage hardware products](http://blog.fosketts.net/2008/11/15/top-ten-storage-hardware/). He invited me to participate in creating the list, but I just didn't feel like I would have been able to contribute anything genuinely useful. Storage is an area I enjoy, but I don't think I've risen to the ranks of "storage guru" just yet.

* And in the area of top 10 storage lists, Marc Farley shares his list of [top 10 network storage innovations](http://www.storagerap.com/2008/11/top-10-storage-innovations.html) as well. I'll have to be honest---I recognize more of these products than I did ones on Stephen's list.

* Robin Harris of StorageMojo provides some great insight into the [details behind EMC's Atmos](http://storagemojo.com/2008/11/12/the-computer-science-behind-emcs-cloud-storage/) cloud storage product. I won't even begin to try to summarize some of that information here as it's way past my level, but it's fascinating reading. What's also interesting to me is that EMC chose to require users to use an API to really interact with the Atmos (more detailed reasons why [provided here](http://virtualgeek.typepad.com/virtual_geek/2008/11/whats-the-relat.html) by Chad Sakac), while child company VMware is seeking to prevent users from having to modify their applications to take advantage of "the cloud." I don't necessarily see [a conflict between these two approaches](http://blog.fosketts.net/2008/11/10/emc-atmos-vmware-vdc-os-cloud-strategy/) as they are seeking to address two different issues. Actually, I see similarities between EMC's Atmos approach and Microsoft's Azure approach, both which require retooling applications to take advantage of the new technology.

* Speaking of Chad, here's a recent post on [how to add storage](http://virtualgeek.typepad.com/virtual_geek/2008/11/celerra-virtual.html) to the Celerra Virtual Appliance.

* Andy Leonard took up [a concern about NetApp deduplication](http://andyleonard.com/2008/10/08/practical-limits-of-netapp-deduplication/) and volume size limits a while back. The basic gist of the concern is that in its current incarnation, NetApp deduplication limits the size of the volume that can be deduplicated. If the size of the volume ever exceeds that limit, it can't be deduplicated---even if the volume is subsequently resized back within the limit. With that in mind, users must actively track deduplication space savings so that, in the event they need to undo the deduplication, they don't inadvertently lose the ability to deduplicate because they exceeded the size limit. Although Larry Freeman [aka "Dr Dedupe"](http://blogs.netapp.com/drdedupe/) responded in the comments to Andy's post, I don't think that he actually addressed the problem Andy was trying to state. Although the logical data size can grow to 16TB within a deduplicated volume, you'll still need to watch deduplication space savings if you think you might need to undo the deduplication for whatever reason. Otherwise, you could exceed the volume size limitations and lose the ability to deduplicate that volume.

* And while we are on the subject of NetApp, a [blog post](http://storage.blogs.techtarget.com/2008/03/19/user-response-about-netapp-and-fc-lun-snapshots/) by Beth Pariseau from earlier in the year recently caught my attention; it was in regards to NetApp Snapshots in LUN environments. I've discussed a little bit of this before in my post about [managing space requirements with LUNs][1]. The basic question: how much additional space is recommended---or required---when using Snapshots and LUNs? Before the advent of Snapshot auto-delete and volume autogrow, the mantra from NetApp was "2x + delta"--two times the size of the LUN plus changes. With the addition of these features, deduplication, and additional thin provisioning functionality, NetApp has now moved their focus to "1x + Delta"--the size of the LUN plus space needed for changes. It's not surprising to me that there is confusion in this area, as NetApp themselves has worked so hard to preach "2x + Delta" and now has to go back and change their message. Bottom line: You're going to need additional space for storing Snapshots of your LUNs, and the real amount is determined by your change rate, how many Snapshots you will keep, and for how long you will keep them. 20% might be enough, or you might need 120%. It all depends upon your applications and your business needs.

* If you're into Solaris ZFS, be sure to have a look at this [NFS performance white paper](http://developers.sun.com/solaris/articles/nfs_zfs.html) by Sun. It provides some good details on recent changes to how NFS exports are implemented in conjunction with ZFS.

That's it for this time around, but feel free to share any interesting links and your thoughts on them in the comments!

[1]: {{< relref "2007-12-05-managing-lun-space-requirements-with-netapp-storage.md" >}}
