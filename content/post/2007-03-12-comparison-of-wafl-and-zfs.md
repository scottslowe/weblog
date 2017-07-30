---
author: slowe
categories: Information
comments: true
date: 2007-03-12T17:30:26Z
slug: comparison-of-wafl-and-zfs
tags:
- NetApp
- ONTAP
- Solaris
- Storage
- UNIX
- WAFL
- ZFS
title: Comparison of WAFL and ZFS
url: /2007/03/12/comparison-of-wafl-and-zfs/
wordpress_id: 427
---

[This comparison](http://uadmin.blogspot.com/2006/07/zfs-vs-netapps-wafl.html) of [ZFS](http://www.sun.com/software/solaris/ds/zfs.jsp) (Zettabyte File System) and WAFL (Write Anywhere File Layout) by [Network Appliance](http://www.netapp.com/) (no direct link for WAFL) is an interesting comparison of these two advanced filesystems and their feature set. Be sure to read the comments for some additional insight on the comparison of the two filesystems and some clarification about supported features.

One distinction raised in the comments that's worthy to be noted here is that any comparison of this sort also, by its very nature, takes into account the operating system that runs the filesystem. As a result, any comparison of ZFS vs. WAFL also involves, to a lesser extent, [Solaris](http://www.sun.com/software/solaris/index.jsp) and [Data ONTAP](http://www.netapp.com/products/software/ontap.html), respectively. Similarly (and I think this may have been pointed out in the comments as well), the underlying hardware used by these filesystems (and their operating systems) also comes into play as well.

For these reasons, it's impossible---in my mind, at least---to perform any sort of apples-to-apples comparison of these two technologies. It's still an interesting article, though.
