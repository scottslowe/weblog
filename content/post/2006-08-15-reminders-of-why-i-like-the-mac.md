---
author: slowe
categories: Rant
comments: true
date: 2006-08-15T19:54:10Z
slug: reminders-of-why-i-like-the-mac
tags:
- Interoperability
- Macintosh
- Networking
title: Reminders of Why I Like the Mac
url: /2006/08/15/reminders-of-why-i-like-the-mac/
wordpress_id: 322
---

I make no secret of my preference for Mac OS X; in the past, I've written about [why I chose to use a Mac][1] and [my preferred Mac OS X-based applications][2], including a fair number of [open source applications for the Mac][3]. I'm also not afraid to speak up about what's wrong with Mac OS X, and to speak out against the misconceptions that many Mac users have.

On Friday, though, I was reminded again of why I love the Mac. At one point in time, here's a quick snapshot of what was going on with my Mac:

* On one virtual desktop (I use [VirtueDesktops](http://virtuedesktops.info/)), I had [Cyberduck](http://cyberduck.ch/), an open source SFTP client, transferring an ISO image to a [VMware ESX Server](http://www.vmware.com/products/vi/esx/) host.

* On another virtual desktop, I had a couple of Remote Desktop sessions open to some [Windows Server 2003 R2](http://www.microsoft.com/windowsserver2003/default.mspx)-based servers (multiple Remote Desktop sessions made possible by [RDC Menu](http://www.xutils.com/rdcmenu/)).

* On that same virtual desktop, I had a nested X session running from a virtualized server running [Solaris 10](http://www.sun.com/software/solaris/), thanks to Mac OS X's X11 support.

* Neatly tucked away on yet another virtual desktop was my web browser, RSS reader, and [Cocoalicious](http://www.scifihifi.com/cocoalicious/), a native [del.icio.us](http://del.icio.us/) client.

* Just a hotkey away was [Quicksilver](http://quicksilver.blacktree.com/), ready to do my bidding---opening applications, manipulating data, composing email messages, etc. (Side thought: I'm getting more into the hang of using Quicksilver...used it today to control iTunes running in the background. Handy!)

* Minimized to the Dock was the Kerberos application, which allowed me to obtain a Kerberos ticket from an Active Directory domain; that ticket then granted me single-sign on to any Windows-based shares in the domain(easily mounted on the desktop with a quick Cmd-K keystroke).

* Finally, a Spotlight window sat there showing me all the documents for a particular project I'm working on right now, neatly gathered together regardless of file system location or file type.

Earlier that afternoon I had mounted an NFS volume hosted on a [NetApp](http://www.netapp.com/) device (again utilizing a Kerberos ticket, so no password was required) and had both NFS and CIFS volumes sitting on the Desktop. This in addition to the RDP sessions to the Windows servers, the X11 session to the Solaris server, SSH sessions to various Linux servers (including SSH tunnels), and all the other various applications running. Honestly, I can't recall being that connected to that many different systems using that many different protocols when I used Windows. (That's not a knock against Windows, just an observation.) I love the Mac!

[1]: {{< relref "2005-06-20-why-i-use-a-mac.md" >}}
[2]: {{< relref "2005-06-20-preferred-mac-os-x-applications.md" >}}
[3]: {{< relref "2005-10-06-open-source-on-mac-os-x.md" >}}
