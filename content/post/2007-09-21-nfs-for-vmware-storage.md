---
author: slowe
categories: Information
comments: true
date: 2007-09-21T20:40:12Z
slug: nfs-for-vmware-storage
tags:
- NAS
- NetApp
- NFS
- Storage
- Virtualization
title: NFS for VMware Storage
url: /2007/09/21/nfs-for-vmware-storage/
wordpress_id: 548
---

I thought that I had blogged here before about using NFS for VMware storage, but it appears that I have not. (I guess that's one of the downfalls of a fairly long-running weblog---you blog about some things too often and not at all about other topics.) In any case, following some of the VMworld breakout sessions last week, NFS is getting a lot more attention these days as the storage protocol for VMware.

A couple of recent blog entries on this topic caught my attention:

[Eisler's NFS Blog - VMware over NFS?](http://storagefoo.blogspot.com/2007/09/vmware-over-nfs.html)  

[Storage - VMware over NFS](http://storagefoo.blogspot.com/2007/09/vmware-over-nfs.html)

Network Appliance seems to be talking the most about NFS for VMware, which kind of makes sense given their history in NFS. I'm using NFS in our lab (which uses NetApp storage systems) and have had nothing but positive experiences thus far. I have not yet had the opportunity to conduct any performance tests, but I do plan to try to work up some numbers on NFS vs. software-based iSCSI. I can't, unfortunately, compare to Fibre Channel as I have no FC infrastructure in the lab (yet).

I'd love to hear feedback from any readers that might be using NFS for VMware storage. What have your experiences been?
