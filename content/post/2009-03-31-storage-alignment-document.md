---
author: slowe
categories: Information
comments: true
date: 2009-03-31T19:27:15Z
slug: storage-alignment-document
tags:
- CLI
- ESX
- HyperV
- NAS
- NetApp
- NFS
- ONTAP
- Storage
- Virtualization
- VMFS
- VMware
title: Storage Alignment Document
url: /2009/03/31/storage-alignment-document/
wordpress_id: 1260
---

NetApp has recently released [TR-3747](http://media.netapp.com/documents/tr-3747.pdf), Best Practices for File System Alignment in Virtual Environments. This document addresses the situations in which file system alignment is necessary in environments running VMware ESX/ESXi, Microsoft Hyper-V, and Citrix XenServer. The authors are Abhinav Joshi (he delivered the [Hyper-V deep dive][1] at Insight last year), Eric Forgette (wrote the Rapid Cloning Utility, I believe), and Peter Learmonth (a well-recognized name from the Toasters mailing list), so you know there's quite a bit of knowledge and experience baked into this document.

There are a couple of nice tidbits of information in here. For example, I liked the information on using `fdisk` to set the alignment of a guest VMDK from the ESX Service Console; that's a pretty handy trick! I also thought the tables which described the different levels at which misalignment could occur were quite useful. (To be honest, though, it took me a couple of times reading through that section to understand what information the authors were trying to deliver.)

Anyway, if you're looking for more information on storage alignment, the different levels at which it may occur, and the methods used to fix it at each of these levels, this is an excellent resource that I strongly recommend reading. Does anyone have any pointers to similar documents from other storage vendors?

[1]: {{< relref "2008-12-01-4198-hyper-v-on-netapp-deep-dive.md" >}}
