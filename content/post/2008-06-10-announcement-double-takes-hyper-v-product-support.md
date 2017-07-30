---
author: slowe
categories: News
comments: true
date: 2008-06-10T13:30:55Z
slug: announcement-double-takes-hyper-v-product-support
tags:
- HyperV
- Microsoft
- Virtualization
- VMware
- Windows
title: 'Announcement: Double-Take''s Hyper-V Product Support'
url: /2008/06/10/announcement-double-takes-hyper-v-product-support/
wordpress_id: 728
---

Today Double-Take Software issued a press release, viewable on [their web site](http://www.doubletake.com/news-events/press-releases/releases/PressRelease-TechEd-HyperV-061008.html), that announces their product support for Microsoft Hyper-V. Quoting from their press release:

>Double-Take Software (NASDAQ: DBTK) today unveiled its protection, availability and migration solutions for Windows Server 2008 Hyper-V here at the Microsoft Tech-Ed conference, booth #809. Support includes real-time physical-to-virtual (P2V) full system replication for migration and failover, and virtual-to-virtual (V2V) replication and failover for maximum uptime. Double-Take Software will release an edition of Double-Take specifically designed for Windows Server 2008 Hyper-V, allowing IT administrators to easily and continuously replicate and failover entire virtual machines between Hyper-V hosts for V2V disaster recovery and remote availability.

I had the opportunity to stop by the Double-Take booth and chat with the guys there about their new product announcements. The product announcement basically breaks down into three major sections:

* The company will release a new version of Double-Take designed specifically for Windows Server 2008 Hyper-V, which allows customers to replicate VMs hosted on Hyper-V to other Hyper-V hosts for disaster recovery (DR) or business continuity (BC) purposes.

* Double-Take will add Hyper-V support to their Virtual Recovery Assistant feature, part of Double-Take for Windows. This will allow customers running Double-Take for Windows to replicate physical machines to VMs hosted on Hyper-V. The guys at Double-Take basically called it "continuous P2V". In that regard, it can be seen as a direct competitor to the PlateSpin PowerConvert product, which also does ongoing P2V for DR/BC purposes. A [press release](http://www.doubletake.com/news-events/press-releases/releases/PressRelease_VRA_052008.html) describing the VRA is available here.

* Finally, Double-Take Software will add Hyper-V compatibility to its GeoCluster product by building on top of Microsoft's geo-cluster support in Windows Server 2008. This would allow for Quick Migration functionality across geographically dispersed sites using replicated storage.

I also talked with Double-Take Software about their recommendations regarding _where_ to run the Double-Take products---inside the VM, or on the host? The answer is, as one would expect, "it depends." Running on the host allows for like-to-like V2V replication; e.g., replicating from one Hyper-V host to another Hyper-V host, or one ESX server to another ESX server. Running inside the guest, on the other hand, allows for replication between dissimilar virtualization platforms. So, customers could replicate from ESX server in their production environment to Hyper-V at their failover site. There are other considerations, of course, but this at least gives some ideas.

If you are at Tech-Ed 2008 this week, stop by booth #809 and have a look at the demo. And, if you see Bob Roudebush, give him a hard time for me. He was supposed to meet with me today at 1PM to discuss the new products, but somewhere along the way our communications got crossed and he wasn't available. So give him a tough time for me. Thanks!
