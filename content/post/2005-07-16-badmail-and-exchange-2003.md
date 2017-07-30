---
author: slowe
categories: Information
comments: false
date: 2005-07-16T21:34:22Z
slug: badmail-and-exchange-2003
tags:
- Exchange
- Messaging
- Microsoft
title: Badmail and Exchange 2003
url: /2005/07/16/badmail-and-exchange-2003/
wordpress_id: 50
---

If you are planning an in-place upgrade of your server running Exchange 2000 to Exchange Server 2003, beware of the Badmail folder. Apparently, during the Exchange Server 2003 setup process, the setup application tries to go back and stamp ACLs (access control lists) on all the objects in the installation directory. This, by default, includes the Badmail directory. If your Badmail directory contains lots of items (which, in an Exchange 2000 installation, it probably does), then this can cause the Setup process to appear to be hung. Microsoft has published [this KB article](http://support.microsoft.com/default.aspx?scid=kb;en-us;822578) discussing the issue and the resolution.

Fortunately, in Exchange Server 2003 SP1, Microsoft has changed the behavior of Exchange to use the Badmail folder only if explicitly configured to do so (see [this KB article](http://support.microsoft.com/default.aspx?scid=kb;en-us;884068)). No more monitoring the Badmail folder!

In addition, for those networks that have not yet deployed Exchange Server 2003 SP1, Microsoft has released the [BadMailAdmin tool](http://support.microsoft.com/default.aspx?scid=kb;en-us;867642). I've tested this, and it works as advertised.
