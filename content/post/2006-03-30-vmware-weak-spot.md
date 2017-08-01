---
author: slowe
categories: Rant
comments: true
date: 2006-03-30T21:01:34Z
slug: vmware-weak-spot
tags:
- Apple
- ESX
- Interoperability
- Linux
- Macintosh
- Microsoft
- Virtualization
- VMware
- Windows
title: VMware Weak Spot
url: /2006/03/30/vmware-weak-spot/
wordpress_id: 213
---

[VMware](http://www.vmware.com/) [ESX Server](http://www.vmware.com/products/esx/) is a great product. From what I understand, version 3.0 (current version is 2.5.2) will be even better. However, in the last few days of working with ESX Server fairly extensively, I have uncovered what (for me) is the weak spot: remote console support.

Both [VMware Workstation](http://www.vmware.com/products/ws/) and [VMware GSX Server](http://www.vmware.com/products/gsx/) (and presumably VMware Server, the replacement for GSX) utilize an "internal" VNC server to provide remote console functionality. What does this mean? Essentially, it means that when you run the VMware Remote Console application (which runs natively on Windows or under X on Linux), you first authenticate (using a connection to TCP port 902, which is opened by the vmware-authd service/daemon), and that connection wraps a VNC session to the virtual machine. There is also a workaround that allows you to connect [directly to the VM using VNC](http://www.vmware.com/support/kb/enduser/std_adp.php?p_faqid=1246). This is kind of handy (although it does have its limitations), since it bypasses the need for the full Remote Console application.

You would think that ESX Server, VMware's flagship product, would also offer the same functionality. After all, ESX Server can do traffic shaping, manage CPU utilization, move virtual machines between hosts with no downtime (using VMotion), and more...why not VNC access to virtual machines? But alas, this is not the case, and therein lies my problem.

There is no native [Mac OS X](http://www.apple.com/macosx/) VMware Remote Console. I have no Windows-based workstation sitting next to me (as I have in days past) in the event I run into an issue, and I have not yet re-installed Microsoft Virtual PC following [my Tiger upgrade][1]. I have no Linux workstation that I can fall back on, either. This leaves me with no way to run a "true" VMware Remote Console session, and therefore no way to boot up new VMs running on ESX Server.

I tried all the usual workarounds---using Remote Desktop to connect to a Windows-based machine and running Remote Console from there, using VNC to connect to a Windows-based machine and running Remote Console---with no success. I even tried the workaround for GSX and Workstation hoping that perhaps it worked with ESX....still no luck. I even tried to run what I though to a be a Remote Console application on a Linux host and push the display back to my PowerBook via X11 and SSH, but that didn't work either.

In my eyes, this is a critical weak spot. More and more technical folks are using Mac OS X, especially as the Intel-based Macs ramp up, and VMware is gaining popularity _very_ rapidly in the tech community. Where's the Mac OS X port? Even a version that ran under X11 on Mac OS X would be acceptable at this point.

In the meantime, I'll continue looking for workarounds. Does anyone know how difficult it is to install the X Window System on the ESX host? Is it recommended? At least then I could try piping the display back to my laptop again.

Anyone with any suggestions, please provide them in the comments. I'll be eternally grateful to the person who shows me a workaround!

[1]: {{< relref "2006-03-12-my-tiger-upgrade.md" >}}
