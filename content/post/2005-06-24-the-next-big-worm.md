---
author: slowe
categories: Musing
comments: false
date: 2005-06-24T22:29:09Z
slug: the-next-big-worm
tags:
- Microsoft
- Security
- Windows
title: The Next Big Worm?
url: /2005/06/24/the-next-big-worm/
wordpress_id: 22
---

Everyone remember Blaster? There are concerns that the next big Internet worm is about to surface, based upon a vulnerability revealed by Microsoft in the Windows implementation of Server Message Block, or SMB. Get the details on that patch [here](http://www.microsoft.com/technet/security/Bulletin/MS05-011.mspx).

Several articles have been posted recently indicating that traffic patterns have been observed that might indicate an exploit of that vulnerability.  [This article](http://www.computerworld.com/securitytopics/security/story/0,10801,102747,00.html) indicates that some experts believe the traffic patterns are indicative of a new exploit, but others are not concerned.

More recent than even that article is [this posting](http://www.eweek.com/article2/0,1759,1831325,00.asp), revealing that an exploit for MS05-011 has actually been discovered and made available on the Internet.

So is there anything you can do to protect yourself? Most firewalls (network-based and host-based) already block TCP port 445, the port used by SMB. If you know your organization allows SMB across the firewall, then you should be concerned. Make sure your systems are patched properly, and keep your anti-virus up to date. That's about all you can do: just stay vigilant.
