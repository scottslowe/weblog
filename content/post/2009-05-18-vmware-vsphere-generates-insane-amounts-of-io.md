---
author: slowe
categories: News
comments: true
date: 2009-05-18T17:47:58Z
slug: vmware-vsphere-generates-insane-amounts-of-io
tags:
- EMC
- ESX
- SAN
- Storage
- Virtualization
- VMware
- vSphere
title: VMware vSphere Generates Insane Amounts of I/O
url: /2009/05/18/vmware-vsphere-generates-insane-amounts-of-io/
wordpress_id: 1367
---

The news has hit the Internet in various places, but I wanted to point it out here because it does help to debunk the myth that virtualization can't handle all workload. What's the news? EMC and VMware have jointly demonstrated that a single VMware vSphere host running just three virtual machines can drive just above **350,000 I/O operations per second (IOPS).**

I'll let that sink in for just a moment. In case you don't understand just how significant that number is, consider that a typical Fibre Channel drive can sustain somewhere just below 200 IOPS (and that's being a bit generous). At 200 IOPS per drive, driving 350,000 IOPS would require 1,750 drives. (Fortunately, EMC used Enterprise Flash Drives (EFDs), so far fewer drives were required.) I would wonder how many of us have actually seen a storage array with that many drives.

Chad Sakac of EMC [covered the tests](http://virtualgeek.typepad.com/virtual_geek/2009/05/update-on-the-io-vsphere-performance-test.html) on his blog here; the VMware Performance blog also [discussed the results](http://blogs.vmware.com/performance/2009/05/350000-io-operations-per-second-one-vsphere-host-with-30-efds.html) in detail as well.

So, next time you are thinking that VMware vSphere can't handle your database workloads, keep these figures in mind. Or, if you're a consultant like me, use these figures next time your customer says that virtualization can't handle I/O-intensive workloads. This looks like pretty definitive results to me.
