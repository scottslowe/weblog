---
author: slowe
categories: Explanation
comments: true
date: 2011-04-27T09:28:28Z
slug: vmware-esxi-local-disks-showing-as-remote
tags:
- Interoperability
- Cisco
- ESXi
- UCS
- Virtualization
- VMware
- vSphere
title: VMware ESXi Local Disks Showing as Remote
url: /2011/04/27/vmware-esxi-local-disks-showing-as-remote/
wordpress_id: 2281
---

A little over a month ago, I was installing VMware ESXi on a Cisco UCS blade and noticed something odd during the installation. I posted [a tweet about the incident](https://twitter.com/#!/scott_lowe/status/51385150763831297). Here's the text of the tweet in case the link above stops working:

>Interesting...this #UCS blade has local disks but all disks are showing as remote during #ESXi install. Odd...

Several people responded, indicating they'd run into similar situations. No one---at least, not that I recall---was able to tell me _why_ this was occurring, only that they'd seen it happen before. And it wasn't just limited to Cisco UCS blades; a few people posted that they'd seen the behavior with other hardware, too.

This morning, I think I found the answer. While reading this post about [scratch partition best practices](http://blogs.vmware.com/esxi/2011/04/scratch-partition-best-practices-for-usbsd-booted-esxi.html) on [VMware ESXi Chronicles](http://blogs.vmware.com/esxi/), I clicked through to a VMware KB article referenced in the post. The KB article discussed all the various ways to set the persistent scratch location for ESXi. (Good article, by the way. Here's a [link](http://kb.vmware.com/kb/1033696).)

What _really_ caught my attention, though, was a little blurb at the bottom of the KB article in reference to examples where scratch space may not be automatically defined on persistent storage. Check this out (emphasis mine):

>2. ESXi deployed in a Boot from SAN configuration or to a SAS device. **_A Boot from SAN or SAS LUN is considered Remote_**, and could potentially be shared among multiple ESXi hosts. Remote devices are not used for scratch to avoid collisions between multiple ESXi hosts.

There's the answer: although these drives are physically inside the server and are local to the server, they are considered remote during the VMware ESXi installation because they are SAS drives. Mystery solved!
