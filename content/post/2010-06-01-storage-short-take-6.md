---
author: slowe
categories: Information
comments: true
date: 2010-06-01T23:00:43Z
slug: storage-short-take-6
tags:
- EMC
- EMCWorld2010
- Storage
title: 'Storage Short Take #6'
url: /2010/06/01/storage-short-take-6/
wordpress_id: 1960
---

Welcome to Storage Short Take #6, the latest installation of my quite irregular collection of storage-related links, posts, and thoughts. You could call this the "EMC World" edition, since most of the items listed here stemmed from announcements that came out of EMC World a few weeks ago.

* One of the announcements from EMC World was FAST Cache, a new feature that EMC is slated to include in the next version of FLARE. Mark Twomey aka Storagezilla has a good write-up of FAST Cache [here](http://storagezilla.typepad.com/storagezilla/2010/05/fast-cache-for-emc-unified-storage.html). The nice thing about FAST Cache is that you don't need new hardware to use it---if you already have a CX4-based array and EFDs, you can use FAST Cache when the next version of FLARE gets released. That's pretty cool.

* Mark also does a great job of describing the storage pool architecture that is the preferred way of provisioning storage in the future. Read [this post on storage pools](http://storagezilla.typepad.com/storagezilla/2010/05/the-clariion-storage-pool.html) and [this post on storage pool services](http://storagezilla.typepad.com/storagezilla/2010/05/storage-services-for-clariion-storage-pool-luns.html) for more information on new features like block compression (leveraging the EMC RecoverPoint compression engine), thick/thin LUNs, and sub-LUN FAST (Fully Automated Storage Tiering). These posts really help provide an idea of where EMC's midrange arrays are headed.

* An EMC Technical Consultant in the Seattle area also [published a list of key changes in FLARE 30](http://storagesavvy.com/2010/05/15/emc-unified-updates/); it's worth a look. Also listed are all the existing features in the midrange storage platforms.

* Chad Sakac also tackled the changes to EMC's midrange platforms in a pair of posts: first a post on [next-generation simplicity](http://virtualgeek.typepad.com/virtual_geek/2010/05/emc-unified-storage-next-generation-simplicity.html) (primarily focused around the new Unisphere management interface), then a post on [next-generation efficiency](http://virtualgeek.typepad.com/virtual_geek/2010/05/emc-unified-storage-next-generation-efficiency-details.html). Both articles are good reads---almost all of Chad's posts are good reads---and worth taking the time to read, understand, and apply. I have to say that Chad's multi-dimensional discussion of efficiency is an excellent overview of how storage features or functions might help in one area (like capacity efficiency) but not necessarily others (such as performance efficiency). This need to be able to offer efficiencies in multiple dimensions is, in my mind, the reason for a broader product set. In other words, the idea of multi-dimensional efficiency is why one size doesn't fit all.

* Here's a list of the ["top ten revelations"](http://www.enterprisestorageforum.com/industrynews/article.php/3883916/Top-Ten-Revelations-from-EMC-World.htm) from the conference.

* Moving away from EMC World-related links, here's a ["quick hit"](http://blog.virtualtacit.com/home/2010/6/1/quick-hit-pause-bb_credits-and-pfc.html) by Joe Kelly on various networking mechanisms for ensuring lossless behavior. It's a pretty good intro to how some of the different mechanisms work.

* Interesting discussion going on over at Dan Weiss' site about a recent instance in which his company (an EMC reseller) was able to retain a customer against a competing NetApp configuration. Go read [the post and the comments](http://blog.dpweiss.com/?p=155) for full details.

* The EMC Storage Info site has a "how to" post on [performing backups/restores using TimeFinder BCV](http://www.emcstorageinfo.com/2010/05/how-to-backuprestore-using-timefinder.html) in Windows.

* RecoverPoint is one of the technologies into which I'm trying to dig deeper, so I've been gathering information from whatever sources I can find. It turns out that the EMC RecoverPoint team has [a YouTube channel](http://www.youtube.com/user/RecoverPoint) with some videos. I'm curious to know---is this something you find useful?

As always, I welcome your feedback on this and other posts, so speak up the comments!
