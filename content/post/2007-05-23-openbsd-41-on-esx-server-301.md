---
author: slowe
categories: Explanation
comments: true
date: 2007-05-23T11:13:01Z
slug: openbsd-41-on-esx-server-301
tags:
- BSD
- ESX
- UNIX
- Virtualization
- VMware
title: OpenBSD 4.1 on ESX Server 3.0.1
url: /2007/05/23/openbsd-41-on-esx-server-301/
wordpress_id: 459
---

Having tested a couple of the previous releases of [OpenBSD](http://www.openbsd.org/) on various versions of [ESX Server](http://www.vmware.com/products/vi/esx/) (OpenBSD 3.8 [here][1] and version 3.9 [here][2]), I decided to test OpenBSD 4.1 on ESX Server 3.0.1. I didn't really expect any problems at first, but then I stumbled across this article describing a [problem between OpenBSD 4.1 and ESX Server 2.5](http://dragonmantank.com/?p=1). Fortunately, it appears as if that problem does not affect ESX Server 3.0.1.

The VM configuration was pretty straightforward; just change the SCSI controller to LSI Logic instead of BusLogic and everything should work beautifully. The [OpenBSD 4.1 release notes](http://www.openbsd.org/41.html) indicate a new driver for the VMware VMXnet NIC driver, but I haven't had the opportunity to test this yet; my installation is still using the pcn0 driver.

After installing OpenBSD, I was able to successfully install a number of packages using `pkg_add` (pulling the packages down via FTP). Not confident that I had transferred enough data to be sure that I wasn't going to see the same issue the other blogger ran into with his installation, I fired up [Interarchy](http://www.nolobe.com/interarchy/) and transferred a 450MB ISO file via SFTP. The file transfer completed successfully and I ran into no problems.

I'll likely experiment with adding the VMXnet driver within the next few days to see how that works and how it affects network performance.

[1]: {{< relref "2006-04-03-openbsd-on-esx-server.md" >}}
[2]: {{< relref "2006-05-18-openbsd-39-on-esx-server.md" >}}
