---
author: slowe
categories: Rant
comments: true
date: 2009-09-22T18:30:26Z
slug: vmfs-isnt-the-problem-here
tags:
- Backup
- Virtualization
- VMware
- vSphere
title: VMFS Isn't the Problem Here
url: /2009/09/22/vmfs-isnt-the-problem-here/
wordpress_id: 1660
---

I was reading [this article by Eric Gray about CSV](http://www.vcritical.com/2009/09/hands-off-that-csv/) and followed his link to [this ZDNet post about VMFS-3](http://blogs.zdnet.com/perlow/?p=11084). I was a little taken aback.

One of the first questions that popped into my head was, "Why not use VMware Converter?" After all, it's the tool that was _expressly designed_ to do exactly what Jason Perlow was attempting to do in the DR exercise with his customer: import data back into a VMware vSphere environment.

Further---and I could be wrong here---but it seems to me that I've accessed USB storage devices from the Service Console before. Assuming that I'm correct (again, a big assumption, and one that I'll test in the lab today), why not just use `vmkfstools` to import the VMDKs back into a VMFS datastore? And yes, Jason does provide a passing reference to ESXi, so perhaps he was using ESXi at the DR site and therefore didn't have access to the Service Console.

I wasn't there in the DR exercise, so there may have been other mitigating circumstances of which I am not aware. In addition, using VMware Converter wouldn't have addressed the lengthy time taken to copy the data across the network which, as I understand it, was one of the primary complaints in Jason's article.

In the end, though, why is Jason complaining about VMFS-3 and the time taken to restore a bunch of VMware vSphere virtual machines when the real root of the problem was the "consumer grade hard disks that you buy at the local Staples or Best Buy hooked up to some random Linux server"? He would have faced the exact same problem if he were trying to restore large Exchange databases from these commodity 1TB hard disks, or if he were trying to restore large Oracle or SQL databases from commodity 1TB hard disks. Does that mean Microsoft and Oracle are at fault as well? In this case, since VMware is the new "whipping boy" in the data center, VMware---specifically, VMFS-3--gets blamed.

I fail to see how better VMFS interoperability would have helped. Even if he'd been able to run VMs off USB-attached consumer grade hard disks, would you have wanted to? Then, VMware vSphere would have been blamed for "awful, horrible performance". Like I said, VMware is the new whipping boy, and everything is VMware's fault. Hey, at least the networking guys are happy---it's no longer the network's fault!

And while this wouldn't have helped in Jason's situation (he was trying to get data into VMFS, not get data out of VMFS), there is [this open source VMFS driver](http://code.google.com/p/vmfs/) available. So VMFS-3 isn't as much the "black box" that he lets on, in my opinion.

Let me know what you think: am I way off here?
