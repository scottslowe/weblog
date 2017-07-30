---
author: slowe
categories: News
comments: true
date: 2008-09-24T09:20:53Z
slug: hyper-v-update-released
tags:
- Hardware
- HyperV
- Microsoft
- Virtualization
- Windows
title: Hyper-V Update Released
url: /2008/09/24/hyper-v-update-released/
wordpress_id: 951
---

I was alerted to this by a friend of mine within the Borg mothership...er, who works at Microsoft. An update has been released for the Hyper-V role of Windows Server 2008 x64 (available for download [here](http://www.microsoft.com/downloads/details.aspx?FamilyID=fe36823a-7e5a-4262-9bf5-d6b3ae3ad375&DisplayLang=en)) that increases the number of processors and virtual machines supported. With the update, Hyper-V can support up to 24 logical processors and up to 192 virtual machines. My contact also tells me that the update doubles the address space cache limit (up to 384 from 192), although I didn't see that documented anywhere.

The [related Microsoft KB article](http://support.microsoft.com/?kbid=956710) for the Hyper-V update also makes mention of a couple of interesting tidbits:

* Although the update does allow Hyper-V to run up to 192 virtual machines, a Registry edit is required if users want to run more than 150 virtual machines.

* If you want to run Windows Server 2008 on a 6-core CPU, you may also have to install hotfix 950182. Apparently, Windows expected CPU core counts to always be powers of 2 (i.e., 2, 4, 8). This update fixes that assumption.

Thanks to my contact for the tip.

**UPDATE:** My contact (need to think up a cool code name for her/him) supplied a link to the documentation on the increase of the address space cache limit. That information is [here](http://blogs.msdn.com/tvoellm/archive/2008/09/24/ws08-hyper-v-now-supports-24lp.aspx).
