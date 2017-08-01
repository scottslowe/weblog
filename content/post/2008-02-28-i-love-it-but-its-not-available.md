---
author: slowe
categories: Rant
comments: true
date: 2008-02-28T21:07:46Z
slug: i-love-it-but-its-not-available
tags:
- NAS
- NetApp
- NFS
- Storage
- VDI
- Virtualization
- VMware
title: I Love It, But It's Not Available
url: /2008/02/28/i-love-it-but-its-not-available/
wordpress_id: 647
---

A friend of mine at [Network Appliance](http://www.netapp.com/) was one of the presenters last year at VMworld 2007 for the now-famous presentation that showed a solution from Network Appliance where 100 VMs are created in just a couple of minutes. It's great technology that is extremely useful in exactly those kinds of situations. I love it.

The video is so popular that it's even been [posted to YouTube](http://youtube.com/watch?v=7Miv0PiJFzM). (By the way, did you know [I'm on YouTube?](http://youtube.com/watch?v=EvqPJNDlGtg) My kids think that's the greatest thing in the world, but I'm not so convinced.)

And, [according to Manlio](http://virtualaleph.blogspot.com/2008/02/optimizing-storage-for-vdi.html), it appears that they are showing off this kind of thing again at VMworld Europe 2008.

But it's technology that's not available yet.

Yep, that's right. It's not available yet. It's based on new functionality, related to their existing FlexClone functionality (which I've [blogged about before][1]), that is due to be released very soon. Combine this new functionality with NFS on a NetApp storage system and you'll be able to do exactly what NetApp is demonstrating. But not today...not until these new features are made available to the public.

That bothers me. I suppose it shouldn't; I mean, you've got all sorts of vendors talking about their products and what their products can do when those products aren't yet available. Microsoft Hyper-V is one example---it's not available yet, won't be until later this year, and yet Microsoft is showing it off. VMware is doing the same thing with the [Continuous HA stuff they demo'ed at VMworld 2007][2]. Likewise, VMware's [done the same thing][3] this year with offline VDI and scalable virtual image technology.

So, if you're thinking about a huge VDI deployment and planning on putting that on NetApp storage, that's fine because there are plenty of other reasons to use Network Appliance---deduplication, anyone? But don't plan on being able to take advantage of some of this highly touted functionality until it is publicly released.

**UPDATE:** Another colleague of mine at NetApp wrote me to clarify that the file-level cloning functionality demonstrated in the video is not, technically speaking, related to FlexClone functionality since FlexClone operates on a per-volume basis. I might argue that they both appear to exploit the same underlying functionality in WAFL, but I don't know that for certain and at that point we're splitting hairs anyway.

[1]: {{< relref "2007-05-11-how-to-provision-vms-using-netapp-flexclones.md" >}}
[2]: {{< relref "2007-09-13-vmworld-2007-day-3-keynote-liveblog.md" >}}
[3]: {{< relref "2008-02-26-vdi-announcements-at-vmworld-europe-2008.md" >}}
