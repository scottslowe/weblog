---
author: slowe
categories: Explanation
comments: true
date: 2008-04-24T10:55:33Z
slug: using-netapp-deduplication-with-block-storage
tags:
- iSCSI
- NetApp
- ONTAP
- SAN
- Storage
title: Using NetApp Deduplication with Block Storage
url: /2008/04/24/using-netapp-deduplication-with-block-storage/
wordpress_id: 692
---

Building on my earlier article on [setting up NetApp deduplication][1], I wanted to follow up with some information on using NetApp deduplication with block storage (LUNs presented via Fibre Channel or iSCSI).

For the most part, using NetApp deduplication with block storage is a lot like I described earlier:

* You (obviously) still need the NearStore and deduplication (A-SIS) licenses installed on the controller(s).

* You will still turn deduplication on using the `sis on` command for the FlexVol containing the LUNs.

* Limitations on the size of the FlexVol still apply.

* You use the `sis status` command to check on the status of deduplication, and the `sis config` command to see the deduplication schedule.

OK, so what's different? Well, it has to do with how LUNs are provisioned on a NetApp storage system. I've blogged before about [managing LUN space requirements on a NetApp][2], and about [using LUN clones vs. FlexClones][3]. That second article, in particular, really goes into detail on how LUNs are implemented on top of NetApp's file system, WAFL. Since LUNs are represented by WAFL as a single file, they are also normally "space reserved," meaning that the maximum size of the LUN is allocated at the time of creation. If you create a 50GB LUN, then Data ONTAP creates a 50GB file right away. (For readers out there who are well-versed in NetApp storage, I know that's a bit of a simplification, but bear with me.)

What does this have to do with deduplication? Great question. If the LUN is space reserved---meaning that the maximum size of the LUN is allocated up front and remains allocated to the LUN---then the file that represents the LUN won't ever decrease in size to reflect deduplication savings, and deduplication therefore does you _absolutely no good whatsoever_. This is not to say that deduplication doesn't work, just that it won't help you at all.

Fortunately, there's an easy fix for this. When creating the LUN, simply uncheck the box marked "Space Reserved" and allow Data ONTAP to allocate space to the LUN out of the containing FlexVol on an as-needed basis. Because the file that represents the LUN can grow in size, it can also shrink in size, and deduplication will cause the file that represents the LUN to decrease in size. This then allows you to provision additional LUNs from the same FlexVol to take advantage of the space savings resulting from deduplication.

I know that seems a bit confusing; I'll probably post another article with some more in-depth discussions of the details. (Either that, or I'll encourage my NetApp readers to chime in below in the comments.)

So, in summary, when using NetApp deduplication with block storage:

* you'll setup and configure deduplication on the FlexVol containing your LUN(s) just like described in my earlier article;

* you'll uncheck the "Space Reserved" checkbox when creating the LUNs to be deduplicated;

* you won't see the space savings from the host's perspective and therefore can't store more data in that LUN than the size of the LUN; but

* you will be able to provision additional LUNs in that same FlexVol that can be presented back to host for additional storage.

I hope this helps clarify some of the questions or issues surrounding the use of NetApp deduplication with block storage. Feel free to add information, experiences with deduplication and block storage, or ask additional questions in the comments below.

**UPDATE:** There are some additional considerations about how to provision LUNs along with NetApp deduplication that warrant a more in-depth discussion. Look for a follow-up post within the next few days.

[1]: {{< relref "2008-03-31-quick-guide-to-setting-up-netapp-deduplication.md" >}}
[2]: {{< relref "2007-12-05-managing-lun-space-requirements-with-netapp-storage.md" >}}
[3]: {{< relref "2007-05-21-lun-clones-vs-flexclones.md" >}}
