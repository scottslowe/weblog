---
author: slowe
categories: Information
comments: true
date: 2006-11-20T17:27:38Z
slug: virtual-consoles-inside-virtual-machines
tags:
- Macintosh
- Virtualization
- VMware
title: Virtual Consoles Inside Virtual Machines
url: /2006/11/20/virtual-consoles-inside-virtual-machines/
wordpress_id: 366
---

Using VMware Fusion (or I suppose that _other_ Intel-based Mac virtualization application, Parallels Desktop), we can now run a Windows instance on our Mac (I'm running it on a Core 2 Duo-based [MacBook Pro](http://www.apple.com/macbookpro/), for example). Like all other VMware hosted products ([VMware Workstation](http://www.vmware.com/products/ws/) or [VMware Server](http://www.vmware.com/products/server/), both on either Windows or Linux), the close-enough-to-native performance allows us to use this instance in order to host an installation of the Virtual Infrastructure (VI) client without an unacceptable performance impact.

OK, that's pretty straightforward, and not too complicated. But what happens when you open a VI console to a virtual machine hosted on your server inside this VM? Then you've got a virtual machine console running inside a virtual machine.

The cool thing about this is not only that it works, and works just fine, but that the software is intelligent enough to recognize the layers of abstraction. For example, today I was running a copy of [Windows XP Professional](http://www.microsoft.com/windowsxp/) under VMware Fusion, and inside that instance of Windows I had the VI client running. With the VI client, I opened a virtual console to a Linux guest running on my ESX server farm. In order to escape out of the Linux console to the Windows VM, I had to press Ctrl-Alt. But that didn't release the VMware Fusion console, which required that I press Ctrl-Cmd. (Please note that this may not work so well when the operating systems are the same in both the guest and the host.)

Of course, this isn't the only workaround for the lack of Mac support in managing ESX server farms; I've heard of people using Remote Desktop Connection (or a similar RDP client) to connect to a Windows instance running on the farm itself. I've tried that, too, but haven't been too pleased with the performance of the VM consoles inside an RDP session. I haven't tested [Citrix](http://www.citrix.com/) yet, but with the recent release of a free 2-user Citrix license I plan to remedy that shortly. I've also read accounts (most notably from Rui Carmo's [Tao of Mac](http://the.taoofmac.com/space/HOWTO/Run%20vmware-console%20Remotely%20With%20Apple%20X11)) of people using X11 to display the consoles on a Mac (even though they are executing elsewhere). Again, I've not tested that so I can't testify as to the performance. I can say that the performance of using a locally hosted VM to access the VI client has been good thus far.

What about you other Mac users out there? What workarounds have you employed (or are employing right now)? I'd love to hear some of your ideas.
