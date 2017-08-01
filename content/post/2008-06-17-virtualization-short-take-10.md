---
author: slowe
categories: Information
comments: true
date: 2008-06-17T23:02:57Z
slug: virtualization-short-take-10
tags:
- ESX
- NFS
- Storage
- Virtualization
- VMware
title: 'Virtualization Short Take #10'
url: /2008/06/17/virtualization-short-take-10/
wordpress_id: 743
---

A lot of virtualization-related articles were published while I was in Orlando at Microsoft Tech-Ed 2008. Here are a few that caught my attention over that time period as well as in the last few days.

* Rich Brambley over at VM /ETC is on a roll with a couple of good articles, one on [VMFS storage sizing for performance](http://vmetc.com/2008/06/10/vmfs-storage-sizing-for-maximum-performance/) and one on [downgrading the VM HAL after P2V](http://vmetc.com/2008/06/11/how-to-p2v-multi-processor-servers-to-uni-processor-vms/). The title for that second article was "How to P2V Multi-processor servers to Uni-processor VMs", and I was hoping that it was a new trick with VMware Converter or some other P2V tool that would actually take care of the HAL for me. Alas, not this time. Rich's article is useful information nevertheless, don't get me wrong. I am curious, though, as to the source of Rich's storage sizing recommendations. Rich, can you share where you derived those recommendations?

* Ryan Arneson shares some of his experiences in [using an X4500 with ZFS as an NFS datastore for ESX](http://blogs.sun.com/rarneson/entry/the_x4500_zfs_and_vmware). Ryan's post reminds readers of the some of the requirements for using NFS with ESX, so readers new to that sort of configuration may find it helpful.

* Gabe brings us, courtesy of [this VMware Communities thread](http://communities.vmware.com/message/969563), some information on [getting DRS VMotion information from the VC database](http://www.gabesvirtualworld.com/?p=69). That's handy. In an earlier article, Gabe also weighed in on [storage sizing](http://www.gabesvirtualworld.com/?p=68) as well. This seems to be getting quite a bit of attention recently (gee, I can't imagine why). Readers, I'd love to hear your thoughts on storage sizing approaches, algorithms, etc. Please share them in the comments!

* I found this article discussing [NFS vs. CIFS for VMware](http://www.oreillynet.com/onlamp/blog/2008/04/nfs_vs_cifs_for_vmware.html). At first I was a bit baffled, but then I realized the author must have been talking about VMware's hosted products like VMware Server not ESX. Anyone else tried this?

* Rick Blythe aka VMwareWolf posted an article about [VM customization failing after VC 2.5 upgrade](http://www.vmwarewolf.com/customization-fails-after-vc-25-upgrade/). Fortunately, the fix is easy; just download and install the correct version of the Sysprep tools and put them in the right location on the VC server. The details are all provided at his site, so check that out.

* The VMware Fusion Team [announced support for running Mac OS X Leopard Server in Fusion 2.0](http://blogs.vmware.com/teamfusion/2008/06/virtual-leopard.html) back during the WWDC, but it seems that Parallels may have beaten them to the punch with [an actually shipping product](http://www.virtualization.info/2008/06/release-parallels-server-10.html).

* Schley Andrew Kutz has released a version of the VI Toolkit for .NET that supports mocking. More information on mocking the VI Toolkit is [available here](http://vitfordotnet.wiki.sourceforge.net/#SimplifiedTesting). I haven't seen any sample taunts yet. (You'll get that in a few minutes.)

In addition, as a follow-up to my [Tech-Ed coverage of the Microsoft Assessment and Planning (MAP) toolkit][1], I found these two articles in my list of "things to blog about":

[Microsoft Assessment and Planning How-To Series: Part 1 (Server Virtualization Candidacy Reporting)](http://blogs.technet.com/mapblog/archive/2008/04/23/how-to-series-for-microsoft-assessment-and-planning-part-1-server-virtualization.aspx)  

[[VIDEO] Microsoft Assessment and Planning How-To Series: Part 1 (Server Virtualization Candidacy Reporting)](http://blogs.technet.com/mapblog/archive/2008/04/24/video-microsoft-assessment-and-planning-how-to-series-part-1-server-virtualization-candidacy-reporting.aspx)

Note also that the beta program for MAP 3.1 is already running; this version will add support for Hyper-V. More information [here](http://blogs.technet.com/mapblog/archive/2008/06/13/teched-2008-announcing-microsoft-assessment-and-planning-toolkit-3-1-beta-release.aspx). (I couldn't remember if I stated anything about this my Tech-Ed session liveblogs.)

That about does it this time around. Thanks for reading!

[1]: {{< relref "2008-06-11-vir361-introducing-map-for-windows-server-2008-hyper-v.md" >}}
