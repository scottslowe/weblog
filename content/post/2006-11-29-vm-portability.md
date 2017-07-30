---
author: slowe
categories: Information
comments: true
date: 2006-11-29T21:36:08Z
slug: vm-portability
tags:
- Macintosh
- Virtualization
- VMware
title: VM Portability
url: /2006/11/29/vm-portability/
wordpress_id: 373
---

The [article at virtualizationdaily.com](http://www.virtualizationdaily.com/archives/73_how-to-convert-a-vmware-virtual-appliance-to-work-with-parallels.html) is pretty cool, but I'd be much more interested if it were written the other way around---converting a Parallels VM to run under VMware. Of course, that's not likely to happen until VMware Fusion hits public beta. If I had a copy of Parallels (or a Parallels VM), I'd try it myself. Anyone out there with a Parallels VM they want to donate? Hey, wait a minute...I could just try one of the [prebuilt Parallels appliances](http://www.virtualizationdaily.com/archives/77_ready-to-run-virtual-appliances-for-parallels-on-mac-os-x-windows-and-linux.html) also made available at virtualizationdaily.com! Stay tuned for more details....

In other VM portability news, I had the opportunity today to try moving a VMware created with [VMware Workstation](http://www.vmware.com/products/ws/) 5.5.x on Windows to VMware Fusion on my [MacBook Pro](http://www.apple.com/macbookpro/). A colleague of mine had built a [Red Hat Linux](http://www.redhat.com/) VM with the [Network Appliance](http://www.netapp.com/) Data ONTAP Simulator, and since I'm out of town in a Network Appliance boot camp, I thought it would be handy to have a copy of that. After all, it would save me the time of installing Red Hat, etc. A quick copy onto a USB 2.0 hard drive and then onto my MBP and I was ready to roll. The VM needed very little tweaking to run; I only had to force NAT networking for both of the VM's network interfaces and everything ran just fine. That bodes _extremely_ well for the future of VMware Fusion and its compatibility (or integration) with VMware Workstation on other host operating systems. As VMware continues to develop and polish this software, it will only get better---and it's not half-bad right now (even for beta software). Today, for example, I had both a Red Hat Linux VM and a [Windows XP Professional](http://www.microsoft.com/windowsxp/) VM up and running and the laptop still didn't even seem to be breaking a sweat. Cool!

The next trick will be to take a VM created with VMware Fusion and move it over to VMware Workstation on Windows or Linux. If I get a chance to try that, I'll post more information here.
