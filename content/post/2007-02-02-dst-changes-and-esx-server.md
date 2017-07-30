---
author: slowe
categories: Information
comments: true
date: 2007-02-02T09:34:33Z
slug: dst-changes-and-esx-server
tags:
- ESX
- Virtualization
- VMware
title: DST Changes and ESX Server
url: /2007/02/02/dst-changes-and-esx-server/
wordpress_id: 408
---

Based on a law signed in 2005 that is scheduled to take effect this year, DST will now start on the second Sunday in March. This has caused some consternation in the IT community as vendors and IT professionals prepare to update servers and network equipment to properly understand the time change.

Fortunately, it appears that [VMware](http://www.vmware.com/) has been pretty proactive in this regard. Based on [this KB article](http://kb.vmware.com/KanisaPlatform/Publishing/224/8695824_f.SAL_Public.html), it looks as if VMware ESX Server 2.5.4 and later (build 32233 or later), 3.0.0 and later (build 27701 or later), and 3.0.1 and later (build 32039 or later) are all already patched for the DST change. This means that as long as you are running a fairly recent version of ESX Server, you're already prepared for the upcoming change.

If you aren't running one of those versions, patches are available to correct the DST change. If you have multiple ESX Servers and VMotion working properly, remember that you can simply migrate guests over to a different ESX host in your farm, patch it, and then migrate the guests back---all with little or no impact to your customers. Isn't VMotion handy?
