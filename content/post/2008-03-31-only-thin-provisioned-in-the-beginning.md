---
author: slowe
categories: Explanation
comments: true
date: 2008-03-31T15:24:19Z
slug: only-thin-provisioned-in-the-beginning
tags:
- ESX
- NAS
- NFS
- Storage
- Virtualization
- VMware
title: Only Thin Provisioned in the Beginning
url: /2008/03/31/only-thin-provisioned-in-the-beginning/
wordpress_id: 670
---

A niggling doubt about thin provisioned disks was placed in my head when I read [Duncan's article on a Storage VMotion problem](http://www.yellow-bricks.com/2008/02/13/storage-vmotion-fails-with-error-message-failed-to-unstun-vm-after-disk-reparent/); in that article, a statement is made that ESX Server 3.5 no longer supports thin provisioned disks. Intrigued by that comment, I started doing some digging to see if that was actually the case. I was unable to find any concrete statement one way or the other.

Some testing in my lab showed that with ESX Server 3.5, VMDKs are still thin provisioned by default when stored on an NFS datastore. So that put to rest the idea that thin provisioned disks had been abandoned, but now I was curious to follow up on the issue of how ESX Server handled cloning thin provisioned disks, as mentioned in [Virtualization Short Take #1][1].

Additional testing showed that although the VMDKs are indeed thin provisioned at the beginning of their life, they won't necessarily stay that way:

* Migration of the VMs files from one datastore to another, even if the destination datastore is also NFS, will cause the VMDKs to revert to thick (fully allocated) VMDKs.

* Clones made from the thin provisioned disks have thick provisioned VMDKs.

* A Storage VMotion operation will cause the disks to become fully allocated instead of thin provisioned.

This is a strong counterpoint to the arguments in favor of using NFS for your VMware storage in order to gain thin provisioned disks. In order to really take advantage of thin provisioned disks, every VM must be provisioned from scratch---no cloning within VirtualCenter---and you must give up Storage VMotion or cold migration of the VMDKs.

So far, I have not found any workaround for this behavior. If anyone knows of a workaround, please share it in the comments. (To be honest, I don't really expect to find one.)

[1]: {{< relref "2008-02-08-virtualization-short-take-1.md" >}}
