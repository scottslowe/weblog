---
author: slowe
categories: Information
comments: true
date: 2007-09-27T23:09:10Z
slug: nifty-nfs-vmware-trick
tags:
- NAS
- NetApp
- Snapshots
- Storage
- Virtualization
title: Nifty NFS-VMware Trick
url: /2007/09/27/nifty-nfs-vmware-trick/
wordpress_id: 550
---

I can take absolutely _zero_ credit for this idea; it came completely from [this aticle by Nick Triantos](http://storagefoo.blogspot.com/2007/09/vmware-on-nfs-backup-tricks.html). But the trick is so absolutely cool, so incredibly useful, and yet so obvious (once you read it, you'll smack yourself in the head and say, "Why didn't I think of that?") that I just had to say something about it.

The use of NFS is getting more and more attention (I blogged about it briefly a few days ago) as a primary storage technology for VMware deployments. Although NFS lacks the raw throughput of Fibre Channel, once you start loading up VMs in a datastore NFS begins to look more and more attractive. But performance is only part of the allure here, especially when using something like a Network Appliance storage system with its Snapshot functionality. (Yes, other vendors can do the same kinds of things. Substitute your favorite vendor or filesystem here, if you so desire. I would imagine you could do something similar with ZFS.)

The basic gist of the article (I do encourage you to go read it; I've already added it to [my del.icio.us bookmarks](http://del.icio.us/slowe/)) is to use NetApp Snapshots to gain access to VMware's VMDK files (even while the VM is running), and Linux with the Linux-NTFS driver to mount virtual machine disk files over NFS for file-level backups of both Windows and Linux guest VMs. Now _that's_ something not even VCB can do (VCB file-level backups are limited to Windows guests). Pretty cool, if you ask me.
