---
author: slowe
categories: Explanation
comments: false
date: 2006-05-18T17:46:58Z
slug: openbsd-39-on-esx-server
tags:
- BSD
- ESX
- OSS
- UNIX
- Virtualization
- VMware
title: OpenBSD 3.9 on ESX Server
url: /2006/05/18/openbsd-39-on-esx-server/
wordpress_id: 252
---

In earlier posts (on the [pcn0 driver in OpenBSD 3.8][1] and on running [OpenBSD 3.8 on VMware ESX Server 2.5][2]) I've provided information on running OpenBSD in a virtualized environment. With the release of [OpenBSD 3.9][3] a few weeks ago, I've completed some testing. Here are the results.

Here's the configuration of the virtual machine under [ESX Server][4] that I used for my testing:

* Guest operating system set to FreeBSD (OpenBSD is not an officially supported guest OS)

* Single CPU (virtual SMP is not supported)

* 128MB of RAM

* LSI Logic SCSI controller (this is a change from the default BusLogic controller)

* Standard vlance network controller

I have not yet tested to see if the BusLogic controllers works under 3.9; it for sure did not work under 3.8 (OpenBSD wouldn't see the disks). If time permits, I will test that soon.

I am very happy to report that the pcn driver now works as expected; it's no longer necessary to disable the pcn driver and use the le driver instead. It is my understanding that the pcn driver is faster and more efficient than the older le driver, so I'm pretty excited that this is now working as expected. My subjective analysis indicates that there is a small performance gain, at least in my environment.

If I run across any additional information, I'll be sure to share it here.

[1]: {{< relref "2005-11-04-openbsd-pcn0-driver-issue-resolved.md" >}}
[2]: {{< relref "2006-04-03-openbsd-on-esx-server.md" >}}
[3]: http://www.openbsd.org/39.html
[4]: http://www.vmware.com/products/esx/
