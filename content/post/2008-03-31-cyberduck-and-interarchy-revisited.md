---
author: slowe
categories: Review
comments: true
date: 2008-03-31T10:06:21Z
slug: cyberduck-and-interarchy-revisited
tags:
- macOS
- Networking
- SSH
title: Cyberduck and Interarchy Revisited
url: /2008/03/31/cyberduck-and-interarchy-revisited/
wordpress_id: 669
---

At the start of 2007, I wrote a [pair][1] of [articles][2] on Mac FTP/SFTP clients, lamenting the performance of the open source [Cyberduck](http://cyberduck.ch/) application and eventually settling on [Interarchy](http://www.nolobe.com/interarchy/) as my replacement product.

Quite a bit has happened since that time, and I now find myself in a bit of a quandary. Cyberduck has replaced the underlying file transfer engine to dramatically improve overall throughput, and Interarchy has "refined" the user interface to make it look more Mac-like. Unfortunately, in the process, Interarchy has become downright ugly. I don't like the ugly chiclet buttons in Leopard's Finder, I don't like them in Safari, and I don't like them in Interarchy, either. I very much prefer "old school" color icons in the toolbars.

Given that the new Interarchy interface was really bugging me---the Finder, at least, is still usable when you turn off the sidebar and close the toolbar---I thought I'd try Cyberduck again. Simple, one-file performance tests showed that Cyberduck, with the option to transfer files via SCP enabled, was coming much closer to matching Interarchy's performance; Interarchy remained in the lead for raw performance, however. I figured that I could live with the performance degradation for most tasks, and started trying to use Cyberduck whenever possible.

Until tonight, that was. I needed to transfer a large number of smaller files up to one of my web sites, and Cyberduck simply dragged. I guess it's the method whereby Cyberduck manages connections that slowed down the process, since it appeared that it checked the connection to the server for each file it was transferring. In any case, I've found that it looks like I'll just have to suffer through Interarchy's new look; the alternative is just too slow.

[1]: {{< relref "2007-01-22-mac-ftpsftp-clients.md" >}}
[2]: {{< relref "2007-01-27-quick-follow-up-on-mac-ftpsftp-clients.md" >}}
