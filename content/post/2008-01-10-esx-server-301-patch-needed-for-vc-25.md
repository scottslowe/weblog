---
author: slowe
categories: News
comments: true
date: 2008-01-10T11:37:42Z
slug: esx-server-301-patch-needed-for-vc-25
tags:
- ESX
- Virtualization
- VMware
title: ESX Server 3.0.1 Patch Needed for VC 2.5
url: /2008/01/10/esx-server-301-patch-needed-for-vc-25/
wordpress_id: 604
---

A notice about a potential interaction between ESX Server 3.0.1 and VirtualCenter 2.5 just shot through my e-mail, and I thought it important enough to post here.

According to [this VMware KB document](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1003401), any ESX Server running version 3.0.1 should have patch ESX-7557441 installed _before_ VirtualCenter is upgraded to version 2.5. If this patch is not installed, it's possible that virtual machines which are configured to automatically start or stop will unexpectedly reboot when VirtualCenter is upgraded to version 2.5. Since VirtualCenter has to be upgraded before you can start upgrading your ESX Servers, then this could definitely be an issue.

More information on why this may occur is provided in [this related VMware KB article](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=7557441).

This may be a fairly rare situation, since (tongue in cheek) organizations always make sure that their ESX Servers are kept patched. (OK, you can stop laughing now.) Seriously, though, if you do still have servers running ESX 3.0.1 be sure to apply this patch before starting along your upgrade path.
