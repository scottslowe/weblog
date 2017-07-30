---
author: slowe
categories: Rant
comments: true
date: 2007-06-02T22:50:32Z
slug: bladecenter-h-woes
tags:
- Networking
- Storage
- Hardware
- IBM
- iSCSI
- NetApp
- Windows
title: BladeCenter H Woes
url: /2007/06/02/bladecenter-h-woes/
wordpress_id: 463
---

This [IBM BladeCenter H](http://www-03.ibm.com/systems/bladecenter/bladeh/index.html) installation I've been working on with another engineer for the last couple of days is not going as smoothly as we both would have liked. I don't know if this is indicative of the BladeCenter H chassis itself, or if it's just me. While some would say it's just me, I suspect it's a little of both.

For example, we ran into issues with the Management Modules "freaking out," and failing over between the modules didn't solve the problem. In fact, we had to power down both management modules and then power them back up one at a time in order to get the fans to finally settle down. Otherwise, the chassis sounded like a jet turbine getting ready to take off, and---get this---people _outside the datacenter_ could hear the chassis.

Even after getting that settled down, there was still some weirdness with the IBM KVM switch that we still haven't resolved (kept freaking out the keyboard and causing it to stop working). We had to plug in a separate USB keyboard in order to work with the chassis. That particular issue still hasn't been resolved.

The real kicker, though, was the problem we ran into with the floppy drive. The design called for boot from SAN via iSCSI (using LUNs off a [Network Appliance](http://www.netapp.com/) storage system) using Qlogic HBAs. This is a supported configuration, but requires the use of a driver floppy during the installation of [Windows Server 2003](http://www.microsoft.com/windowsserver/default.mspx). The older BladeCenter chassis had a built-in floppy, but the BladeCenter H does not, and the USB floppy that we tried to use wouldn't work. No matter how hard we tried, the blade(s) just wouldn't see the floppy drive. Until we can get a floppy drive recognized during the Windows setup process, we can't get Windows installed and we are just _stuck_.

Anyone else run into similar issues with a BladeCenter H?
