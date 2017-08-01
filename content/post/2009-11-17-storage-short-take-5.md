---
author: slowe
categories: Information
comments: true
date: 2009-11-17T12:35:50Z
slug: storage-short-take-5
tags:
- EMC
- FCoE
- HP
- SAN
- Storage
- Virtualization
- vSphere
title: 'Storage Short Take #5'
url: /2009/11/17/storage-short-take-5/
wordpress_id: 1742
---

I've decided to resurrect my Storage Short Take series, after almost a year since the last one was published. I find myself spending more and more time in the storage realm---which is completely fine with me---and so more and more information coming to me in various forms is related to storage. While I'm far from the likes of storage rockstars such as [Robin Harris](http://storagemojo.com/), [Stephen Foskett](http://blog.fosketts.net/), [Storagebod](http://storagebod.typepad.com/), and others, hopefully you'll find something interesting and useful here. Enjoy!

* This [blog post by Frank Denneman on the HP LeftHand product](http://frankdenneman.wordpress.com/2009/10/11/lefthand-san-lessons-learned) is outstanding. I learned more from this post than a _lot_ of posts recently. Great work Frank!

* Need a bit more information on FCoE? Nigel Poulton has a great post [here](http://blogs.rupturedmonkey.com/?p=486) (it's a tad bit older, but I've just stumbled across it) with good details for those who might not be familiar with FCoE. It's worth a read if you haven't already taken the time to come up to speed on FCoE and its "related" technologies.

* What led me to Nigel's FCoE post was [this post by Storagezilla](http://storagezilla.typepad.com/storagezilla/2009/09/fcoe-idiots-to-the-left-of-me-liars-to-the-right-stuck-in-the-middle-with-users.html) in which he rants about "vendor flapheads" who "are intentionally obscuring it's [FCoE's] limitations". You've got that right! Wanting to present a reasonably impartial and complete view of FCoE was partially the impetus behind [my end-to-end FCoE post][1] and the subsequent [clarification][2]. Thankfully, I think that the misinformation around FCoE is starting to die down.

* This post has a bit of useful information on [HP EVA path policies and vSphere multipathing](http://vknowledge.wordpress.com/2009/10/11/vsphere-hp-eva-and-path-policies/). I would have liked a bit more detail than what was provided, but the content is good nevertheless.

* Devang Panchigar's [recoup of HP TechDay day 1](http://storagenerve.com/2009/09/30/hptechday-2009-day-1-hp-storageworks-technology/), which focused on HP StorageWorks technologies, has some good information, especially if you aren't already familiar with some of HP's various storage platforms.

* Chad Sakac of EMC has some [very useful information](http://virtualgeek.typepad.com/virtual_geek/2009/09/a-couple-important-alua-and-srm-notes.html) on Asymmetric Logical Unit Access (ALUA), VMware vSphere, and EMC CLARiiON arrays. If you using EMC storage with your VMware vSphere 4 environment, _and_ you have a CX4, _and_ you're running FLARE 28.5 or later, it might be worthwhile to switch your path policy from NMP to Round Robin (RR).

* Speaking of RR with vSphere, somewhere I remember seeing information on changing the default number of I/Os down a path, and tweaking that for best performance. Was that in Chad's VMworld session? Anyone remember?

* If you're looking for a high-level overview of SAN and NAS virtualization, [this InfoWorld article](http://www.infoworld.com/d/virtualization/deep-dive-san-and-nas-virtualization-241?page=0,0) can help you get started. You'll soon want to delve deeper than this article can provide, but it's a reasonable starting point, at least.

That's it for this time around. Feel free to share other interesting or useful links in the comments.

[1]: {{< relref "2009-07-29-no-such-thing-as-an-end-to-end-fcoe-solution.md" >}}
[2]: {{< relref "2009-07-30-correction-there-might-be-an-end-to-end-fcoe-solution.md" >}}
