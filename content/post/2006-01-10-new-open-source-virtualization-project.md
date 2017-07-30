---
author: slowe
categories: News
comments: false
date: 2006-01-10T20:32:39Z
slug: new-open-source-virtualization-project
tags:
- Networking
- OSS
- Virtualization
title: New Open Source Virtualization Project
url: /2006/01/10/new-open-source-virtualization-project/
wordpress_id: 156
---

[SWSoft](http://www.swsoft.com/) has released [OpenVZ](http://openvz.org/), an open source version of its commercial product [Virtuozzo](http://www.swsoft.com/en/products/virtuozzo/). It appears as if OpenVZ is the "core" of the Virtuozzo product, according to this [NewsForge article describing the release](http://mobile.newsforge.com/article.pl?sid=06/01/09/2050233) of OpenVZ.

Unlike other virtualization products such as [VMware](http://www.vmware.com/) or [Microsoft VirtualPC](http://www.microsoft.com/windows/virtualpc/default.mspx), OpenVZ (and Virtuozzo) are more server partitioning products than full-blown virtualization products. OpenVZ is, at its most basic, a new Linux kernel that allows for multiple instances of Linux to run at the same time. It does not allow [Windows](http://www.microsoft.com/windows/) or [OpenBSD](http://www.openbsd.org/) to run on Linux, instead allowing only multiple instances of the same OS as is running on the host.

This approach has some advantages, such as lower overhead (since it doesn't have to emulate the hardware and run multiple instances of the full code of the OS), but it also has some disadvantages (such as not being able to run different operating systems as guests). Given the differences between "mainstream" virtualization products like VMware and VirtualPC and OpenVZ/Virtuozzo, I wouldn't lump them together as competitors. Rather, they are more like parallel approaches toward the same end goal.
