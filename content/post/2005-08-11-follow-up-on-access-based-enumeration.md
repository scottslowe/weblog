---
author: slowe
categories: Review
comments: false
date: 2005-08-11T21:53:54Z
slug: follow-up-on-access-based-enumeration
tags:
- Microsoft
- Networking
- Windows
title: Follow Up on Access-Based Enumeration
url: /2005/08/11/follow-up-on-access-based-enumeration/
wordpress_id: 74
---

As a follow-up to [this posting]({{< relref "2005-07-08-access-based-enumeration.md" >}}) on [access-based enumeration](http://www.microsoft.com/windowsserver2003/techinfo/overview/abe.mspx), I wanted to post information from some testing I performed earlier this week. I installed access-based enumeration (ABE) on a server running [Windows Server 2003](http://www.microsoft.com/windowsserver2003/default.mspx) SP1 (the only version of Windows on which it is supported). It does what it advertises; when a non-administrative user connects to a shared folder on which ABE is enabled, they see only those folders to which he or she has permission.

There are some limitations. Administrative users are not affected in any way. ABE only works for network access; users accessing the filesystem locally are not affected. This makes ABE unsuitable for use on terminal servers. Additionally, ABE is enabled or disabled on a share-by-share basis, and while it is possible to turn ABE on or off for all shares at a time there is no provision for setting ABE on by default for new shares. Finally, ABE can negatively impact performance, especially for shared folders with large numbers of files.

I'm kind of split on this one. On the one hand, it's really good functionality that will dramatically change the way system administrators approach Windows-based file servers. On the other hand, the potential performance drawbacks of ABE are concerning.

If you are running file servers on Windows Server 2003 with Service Pack 1 installed, you owe it to yourself to at least evaluate ABE. You may find that it doesn't work for your network, but then again you may find that you don't want to go on without the added functionality that ABE brings to the table.
