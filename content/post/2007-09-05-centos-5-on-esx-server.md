---
author: slowe
categories: Information
comments: true
date: 2007-09-05T18:38:36Z
slug: centos-5-on-esx-server
tags:
- CentOS
- ESX
- Linux
- Virtualization
- VMware
title: CentOS 5 on ESX Server
url: /2007/09/05/centos-5-on-esx-server/
wordpress_id: 519
---

As I had to rebuild some Linux VMs in the lab anyway (they'd gotten messed up with various interoperability tests), I decided to try version 5.0 of [CentOS](http://www.centos.org/). I was trying to get this done on Friday before the long holiday weekend, but it turns out that downloading the ISO images for six (yes, six!) CD-ROMs takes a bit longer than I had hoped. So I downloaded them at home on my 6Mbit DSL connection over the weekend, and had the opportunity to work on it yesterday while at the office.

I didn't really expect any major issues that would prevent the new version of the distribution from running, but you never really know for sure until you try. Fortunately, CentOS 5.0 loaded quickly and without any real problems on the lab servers running [ESX Server](http://www.vmware.com/products/vi/esx/) version 3.0.2. The only issue that came up---and it was a minor one, really, more caused by my own lack of preparation than anything else---was that the VMware Tools did not have a suitable kernel module or a suitable VMXNet driver. This was only a problem at first because I didn't have the right parts (compiler, kernel header files, etc.) installed to allow the VMware Tools installer to create its own module and driver. I had to use yum to install the necessary pieces, then the VMware Tools installer worked without any further problems.

Later in the week I hope to be able to post some information on running Solaris 10 8/07 (aka Update 4) on ESX Server as well.
