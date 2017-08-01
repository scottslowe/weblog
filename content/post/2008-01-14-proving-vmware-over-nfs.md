---
author: slowe
categories: Rant
comments: true
date: 2008-01-14T09:51:56Z
slug: proving-vmware-over-nfs
tags:
- ESX
- NAS
- NFS
- Storage
- Virtualization
- VMware
title: Proving VMware Over NFS
url: /2008/01/14/proving-vmware-over-nfs/
wordpress_id: 605
---

I'm a fan of using NFS for VMware; I've [mentioned it before][1]. I'm not the only one, either; there have been a number of recent blog entries from various people regarding the use of NFS for VMware:

[Virtual Optics: Why VMware over NetApp NFS](http://viroptics.blogspot.com/2007/11/why-vmware-over-netapp-nfs.html)

[Storage Nuts & Bolts: VMware over NetApp NFS: A Customer's Testimonial](http://blogs.netapp.com/storage_nuts_n_bolts/2008/01/vmware-over-net.html)

It's great that this is receiving more attention in the spotlight, but I still have one question: where are the statistics _proving_ NFS' value in VMware deployments?

If NFS is equally as good as Fibre Channel or iSCSI---and personally I agree with Nick that most deployments would be hard-pressed to tell the difference---then where are the stats that show this? Or is it impossible to demonstrate that a VMware deployment on NFS is "just as good" as one on Fibre Channel or iSCSI? Does the value of NFS come in subjective measurements that can't be quantified, and perhaps that's why we haven't seen any hard proof of the value of NFS when compared to Fibre Channel or iSCSI?

If you have some insight, please share it in the comments. I'd love to hear everyone's thoughts on the matter.

[1]: {{< relref "2007-09-21-nfs-for-vmware-storage.md" >}}
