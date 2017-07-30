---
author: slowe
categories: Information
comments: false
date: 2006-04-25T22:18:21Z
slug: windows-server-update-services
tags:
- Microsoft
- Security
- Windows
title: Windows Server Update Services
url: /2006/04/25/windows-server-update-services/
wordpress_id: 233
---

The next version of Software Update Services---now renamed as [Windows Server Update Services](http://www.microsoft.com/windowsserversystem/updateservices/default.mspx) (WSUS)---is a pretty significant change from the previous version. And while it may take a little bit of getting used to for users of SUS, the changes are, in my opinion, worthwhile.

There are loads of new features, but the key feature seems to be the enhanced level of communication between the automatic update agent built into Windows and the WSUS server. This new level of communication enables such features as:

* A new "Detect Only" approval for determining which clients need which updates

* The ability to see which clients experienced errors installing an update

* The ability to see which updates a client already has installed

In addition, the interface has received some much-needed updates, such as the ability to filter updates (by approval status, update date, etc.). This makes it much easier to manage updates and update approvals than with the first version.

It's not without its caveats, however. The administrative interface is a web-based interface, and I'm not a huge fan of web-based interfaces. To make it worse, the administrative interface requires Internet Explorer 5.0 or later on Windows. No other browser or operating systems allowed here, unfortunately. For me, that's a big negative. For heavily Microsoft-oriented organizations, this may not be a big deal.

Overall, organizations using SUS to help with distributing patches and updates have very few reasons _not_ to upgrade to WSUS.
