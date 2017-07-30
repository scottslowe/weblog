---
author: slowe
categories: News
comments: true
date: 2006-11-14T17:28:19Z
slug: security-fixes-for-esx
tags:
- ESX
- Security
- Virtualization
- VMware
title: Security Fixes for ESX
url: /2006/11/14/security-fixes-for-esx/
wordpress_id: 362
---

The Secunia advisories ([ESX 2.x here](http://secunia.com/advisories/22875/) and [ESX 3.0.0](http://secunia.com/advisories/22876) here) are dated today and were brought to my attention via [Thincomputing.net](http://www.thincomputing.net/newsitem2786.html). Updates are available for both ESX 2.x (2.0.2, 2.1.3, 2.5.3, and 2.5.4 all have updates available) as well as for ESX 3.0.0 (please note that ESX 3.0.1 is _not_ affected by the same vulnerability).

More information on the fixes (and the associated flaws or vulnerabilities) can be found in the original VMware advisories:

[VMware ESX Server 2.0.2 Upgrade Patch 2](http://www.vmware.com/download/esx/esx-202-200610-patch.html)  
[VMware ESX Server 2.1.3 Upgrade Patch 2](http://www.vmware.com/download/esx/esx-213-200610-patch.html)  
[VMware ESX Server 2.5.3 Upgrade Patch 4](http://www.vmware.com/download/esx/esx-253-200610-patch.html)  
[VMware ESX Server 2.5.4 Upgrade Patch 1](http://www.vmware.com/download/esx/esx-254-200610-patch.html)  
[ESX Server 3.0.0 Patch ESX-2533126](http://kb.vmware.com/vmtnkb/search.do?cmd=displayKC&docType=kc&externalId=2533126&sliceId=SAL_Public)

Here is where the beauty of [VMotion](http://www.vmware.com/products/vi/vc/vmotion.html) comes into play. So you've got a farm of ESX servers running a boatload of virtual servers, and need to take the ESX hosts down to apply this patch, right? No problem...just VMotion the virtual machines on one host off to another host, apply the patch, and reboot the host. When that host comes back up, repeat the process with another host, and then another, until all your ESX servers are patched. The users will never even know that anything was happening.
