---
author: slowe
categories: Information
comments: true
date: 2006-10-24T22:19:20Z
slug: nexenta-alpha-6-on-esx-server
tags:
- Debian
- ESX
- Linux
- Solaris
- Ubuntu
- UNIX
- Virtualization
title: Nexenta Alpha 6 on ESX Server
url: /2006/10/24/nexenta-alpha-6-on-esx-server/
wordpress_id: 349
---

[Nexenta](http://www.gnusolaris.org/gswiki/Nexenta_OS) (also called GNU/OpenSolaris) is a blend of [OpenSolaris](http://www.opensolaris.org/os/), [GNU](http://www.gnu.org/), and [Debian](http://www.debian.org/) ([Ubuntu](http://www.ubuntu.com/), specifically). It's pretty cool, actually---blending the OpenSolaris kernel with Ubuntu userland binaries to create something that's not quite [Solaris](http://www.sun.com/software/solaris/) and not quite Linux, but has some of the values of both. For those of you interested in running it on [VMware ESX Server](http://www.vmware.com/products/vi/esx/), I'm happy to report that it does work just fine.

To install Nexenta as a VM in ESX, I used the following settings:

* 512 MB of RAM

* 4GB virtual disk (pre-allocated); obviously, you would want more space if wanted to do anything useful with Nexenta

* LSI Logic SCSI controller

* "Flexible" network adapter

The installation went very smoothly and very quickly (quicker than Solaris 10 and a couple of the other Linux distributions I've tried on ESX). The system came up very smoothly and was immediately accessible across the network. I didn't try anything useful or meaningful with it; it _is_ an alpha version, after all.

It's worth keeping an eye on, at least. I'll be interested to see how it develops.
