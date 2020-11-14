---
author: slowe
categories: Rant
comments: true
date: 2007-06-03T11:16:53Z
slug: data-ontap-upgrade
tags:
- macOS
- NetApp
- ONTAP
- Storage
title: Data ONTAP Upgrade
url: /2007/06/03/data-ontap-upgrade/
wordpress_id: 464
---

To be honest, there are times when using [Mac OS X](http://www.apple.com/macosx/) can be difficult. With the overwhelming installed base of Windows and the attention of the media darling Linux, vendors will often provide instructions on how to do something with Windows or Linux, but not from Mac OS X. Sometimes the Linux instructions work, but many times they don't, and the Windows instructions typically involve running some sort of Windows executable that won't, of course, run on Mac OS X. What is one to do, then?

This is situation in which I found myself last week while trying to upgrade a [Network Appliance](http://www.netapp.com/) storage system (an old F840) in the lab. I wanted to upgrade the F840 to [Data ONTAP](http://www.netapp.com/products/enterprise-software/storage-system-software/storage-operating-systems/ontap-7g.html) 7.1.2.1, the latest GA release in the 7.1.x family (Data ONTAP 7.2.x isn't supported on the F8xx series). There are instructions for performing the upgrade from a UNIX/Linux-based system as well as instructions for upgrading the storage system from a Windows-based computer. OK, but what about a Mac?

Well, Mac OS X is based on [FreeBSD](http://www.freebsd.org/), so the UNIX instructions should work, right? Well, not quite. Although I mounted the NetApp's root volume via NFS and attempted to extract the files per the instructions, it kept failing. OK, what if I mount it via CIFS? Same result---an error during the file extraction/file copy process and an error when running the "download" command.

Well, I can't exactly run the Windows executable version of the Data ONTAP upgrade because I don't run Windows. The UNIX/Linux instructions don't work. Fortunately, there's a third option---the HTTP server upgrade. I had an OpenBSD-based web server in the lab, so within just a few minutes I had the Data ONTAP upgrade files on that server and then onto the storage system itself using the "software get" command. The process worked quite smoothly, and I was finished upgrading the storage system a few minutes later.

I even went back and reviewed the upgrade guide again to see if it explicitly listed specific client operating systems as a prerequisite for upgrading the storage system. The upgrade guide states that "any available version of UNIX" is required. So why didn't Mac OS X work? Why did I keep getting errors running the Perl install script, yet the HTTP upgrade worked flawlessly? Don't get me wrong, I'm glad the upgrade worked. I just a bit peeved that something that should have worked according to the documentation didn't.

So, here's a tip for all of you out there: if you ever need to upgrade a Network Appliance storage system to a newer version of Data ONTAP from a Mac OS X system, be sure to use the HTTP upgrade method.
