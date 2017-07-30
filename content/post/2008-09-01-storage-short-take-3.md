---
author: slowe
categories: Information
comments: true
date: 2008-09-01T12:59:05Z
slug: storage-short-take-3
tags:
- EMC
- Hardware
- HP
- NetApp
- SAN
- Storage
title: 'Storage Short Take #3'
url: /2008/09/01/storage-short-take-3/
wordpress_id: 847
---

With a collection of various storage-related links from the past few weeks, here's Storage Short Take #3!

* There's a "blog war" brewing between EMC, HP, and NetApp regarding storage virtualization. EMC's Chuck Hollis seems to have started it with [this blog entry](http://chucksblog.typepad.com/chucks_blog/2008/07/the-great-data.html) in which he claims that storage arrays that provide storage virtualization---like the HP EVA and the NetApp FAS series---have inherent performance problems due to data placement. NetApp fired back in the comments to Chuck's article as well as with a [blog entry here](http://blogs.netapp.com/extensible_netapp/2008/07/chuck-almost-ge.html); HP also responded, [available here](http://www.communities.hp.com/online/blogs/datastorage/archive/2008/08/07/data-placement-who-s-architecture-is-really-broken.aspx). Naturally, NetApp and HP both indicated that Chuck was wrong in his assumptions, and that it was EMC who would be facing problems soon. It's an interesting discussion, but one that is way above the likes of mere mortals such as myself. Anyone else care to weigh in?

* And while we are discussing storage blog wars, HP is [stepping up the attack](http://www.communities.hp.com/online/blogs/datastorage/archive/2008/08/11/deduplication-online-storage-and-cannibals.aspx) against NetApp's deduplication functionality. Good luck with that one, HP---NetApp is eating other storage vendors' lunches in VDI deployments because of deduplication. Regardless of whether's NetApp implementation is right or wrong, the door has been opened for deduplication on primary storage. Customers want it. It's up to the rest of the storage community to figure out how to compete with NetApp. If their approach truly is wrong, then prove it. Build the right implementation.

* I had an interesting talk with some guys from [Xiotech](http://www.xiotech.com/) a few days ago regarding their Emprise storage arrays, built on their Intelligent Storage Element (ISE) technology. I won't go into any great detail, but honestly I don't know how much of what they shared with me is confidential (probably none of it, but I'd rather play it safe). I will say this: if half of what they say is true, this is a really compelling solution. As I get more information, I'll share it here.

* I don't really know if this is more virtualization or storage, but a few weeks ago EMC's Chad Sakac blogged about EMC's ability to [quickly provision VMs based on writable pointer-based snapshots](http://virtualgeek.typepad.com/virtual_geek/2008/07/array-snapshots.html). This is pretty cool stuff; I've blogged about NetApp's similar technologies in the past as well. Unfortunately, both of these technologies suffer from the same fate: a lack of direct integration with VMware Infrastructure. EMC is in the potentially beneficial position of...well, owning VMware. We'll see if that turns into an advantage or not.

* Mario Apicella of InfoWorld thinks that [VMotion and FCoE are a match made in admin heaven](http://weblog.infoworld.com/storageadviser/archives/2008/08/take_two_on_fco.html). What do _you_ think?

* I also saw some information on the [Atrato Velocity1000](http://www.atrato.com/Product/Velocity1000.asp) storage array, featuring their "Self-maintaining Array of Independent Disks" (SAID) technology. This looks remarkably similar to Xiotech's technology. Both of them are claiming numerous U.S. patents  that are unique to their products. Can you say "patent lawsuit"?

That wraps up this installation of Storage Short Takes. Feel free to add any interesting news, tips, tricks, or other information in the comments.
