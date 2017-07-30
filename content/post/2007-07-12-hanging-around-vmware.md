---
author: slowe
categories: Information
comments: true
date: 2007-07-12T23:18:37Z
slug: hanging-around-vmware
tags:
- Collaboration
- Virtualization
- VMware
title: 'Hanging Around #vmware'
url: /2007/07/12/hanging-around-vmware/
wordpress_id: 489
---

I'm not really sure why, but for some reason I got on a kick to start using IRC. So, for the last week or so, I've been regularly logging in to irc.freenode.net using Colloquy (a really great Mac OS X IRC client, by the way) and hanging around in the #vmware channel, helping people with their VMware-related problems and questions---and learning a little bit in the process.

Earlier this week I had someone who was trying to export a VM from [ESX Server](http://www.vmware.com/products/vi/esx/) to [VMware Workstation 6.0](http://www.vmware.com/products/ws/) using [VMware Converter](http://www.vmware.com/products/converter/) and kept getting an "guest OS not recognized" error. At first, we thought it might be a problem with a missing BOOT.INI, an incorrectly configured BOOT.INI, or a missing MSDOS.SYS file (yes, even newer versions of Windows have that file, although it is an empty zero-byte file); any of these problems can cause that particular error message. As it turns out, it wasn't any of these issues. The problem was running Converter on [Windows Vista](http://www.microsoft.com/windowsvista/); as soon as the user loaded Converter on a [Windows XP Professional](http://www.microsoft.com/windowsxp/) system, it worked just fine. Go figure.

In another instance, there was a user who wanted to install a custom-compiled Linux kernel in the Service Console of his ESX Server. While I couldn't find any hard and fast rules from VMware on that, I strongly encouraged him to avoid that path. I suspect going down that road would have rendered his ESX Server unusable.

Finally, just earlier tonight I helped someone get a physical installation of Microsoft Windows running as a virtual machine under VMware Server on Ubuntu Linux. That was pretty cool...I'd never done that before. It turns out that this won't work with default permissions in Ubuntu; you have to either run as root (which is disabled by default in Ubuntu) or add your user account to the "disk" group. This is the only way to have permission to the `/dev/hd*` device that represents the physical installation of Windows.

Any other #vmware users out there? I'd love to hear some of your stories from others you've helped and lessons you've learned.
