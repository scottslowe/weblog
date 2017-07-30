---
author: slowe
categories: Explanation
comments: true
date: 2007-10-11T14:48:44Z
slug: disabling-rdp-clipboard-redirection
tags:
- Microsoft
- VDI
- Virtualization
- VMware
- Windows
title: Disabling RDP Clipboard Redirection
url: /2007/10/11/disabling-rdp-clipboard-redirection/
wordpress_id: 559
---

In the event you need to disable clipboard redirection and can't (or don't want to) use Group Policy within Active Directory, there are settings you can add to the default .RDP file for the Remote Desktop Connection client software that will prevent clipboard redirection.

In my particular case, I was working on a VDI project and we wanted to be sure that the connection broker (Leostream) disabled clipboard redirection for all hosted desktops. The broker already supplied some basic settings for the RDP connections, but disabling clipboard redirection wasn't there. Fortunately, I found a site that had a [listing of RDP file parameters](http://www.tech-archive.net/Archive/Windows/microsoft.public.windows.terminal_services/2006-12/msg00363.html) (not from Microsoft, surprisingly!). The listing isn't annotated with explanations of each setting, but they are---for the most part---pretty self-explanatory.

To disable clipboard redirection, add the following to the .RDP file:

	redirectclipboard:i:0

For the ActiveX RDP control, you would use this text:

	MsRdpClient.AdvancedSettings2.RedirectClipboard = FALSE

I guessed on the ActiveX control configuration, but it worked. I haven't found that information documented anywhere just yet.

There are also Registry changes that can be made on the client to disable clipboard redirection (see [here](http://groups.google.com/group/microsoft.public.windows.terminal_services/browse_thread/thread/c9a0ee3ec7055aad/d162ba8da012b303%23d162ba8da012b303)). And, as I mentioned at the beginning of this post, you can also use Group Policy.
