---
author: slowe
categories: Information
comments: true
date: 2006-11-29T22:17:51Z
slug: solaris-10-x86-on-mac-os-x
tags:
- ESX
- Macintosh
- NetApp
- Solaris
- Virtualization
- VMware
title: Solaris 10 x86 on Mac OS X
url: /2006/11/29/solaris-10-x86-on-mac-os-x/
wordpress_id: 374
---

I haven't had the chance to finish installing Solaris 10 yet, as I ran out of time trying to copy down the ISO images before I left to go to Atlanta for some [Network Appliance](http://www.netapp.com/) training. (By the way, have I mentioned how cool NetApp's stuff is? No? It is.)

After first running into the problem with the Solaris 10 VM, I inquired with other private beta testers and later filed a bug report. Another user suggested I check [this KB article](http://kb.vmware.com/vmtnkb/search.do?cmd=displayKC&docType=kc&externalId=1975&sliceId=SAL_Public); I tried adding the monitor\_control.disable\_longmode parameter to the VMX file and....it worked! Where Solaris had previously attempted to load a 64-bit kernel (and caused a host kernel panic) it now loaded a 32-bit kernel and everything seemed to work just fine.

I hope to finish the Solaris installation within the next couple of days and continue on with some additional testing.

**UPDATE:** I finished the Solaris installation earlier today and it runs just fine. I haven't installed the VMware Tools for Solaris yet, but based on my work with Solaris on ESX Server I don't expect any problems there.
