---
author: slowe
categories: Information
comments: true
date: 2009-10-31T15:21:17Z
slug: vm-hardware-version-7-and-windows-server-2008
tags:
- Microsoft
- Virtualization
- VMware
- vSphere
- Windows
title: VM Hardware Version 7 and Windows Server 2008
url: /2009/10/31/vm-hardware-version-7-and-windows-server-2008/
wordpress_id: 1718
---

Stu over at vInternals posted [an article](http://vinternals.com/2009/10/virtual-hardware-7-bug-woeful-vmware-response/) a couple of days ago about a problem he encountered with VMware vSphere and Windows Server 2008. Apparently, there is an unexpected behavior with Windows Server 2008 and VM hardware version 7 that is described in [this VMware KB article](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1013109). Stu, however, was seeing the behavior not on upgrading VMs from VM hardware version 4 to VM hardware version 7, but on new virtual machines created from the beginning with VM hardware version 7.

According to an update on Stu's article, VMware has acknowledged this as a bug and will be investigating a fix to the problem. Until then, follow Stu's advice and speak to your VMware account team if you are experiencing this problem. If you are getting ready to proceed with a VMware vSphere upgrade and have Windows Server 2008 Enterprise Edition VMs in place, keep this behavior in mind and plan accordingly.

Thanks to Stu for bringing this matter to light!

**UPDATE:** Stu posted [an update with more information](http://vinternals.com/2009/11/virtual-hardware-7-undocumented-feature-revealed/) and an explanation for the unexpected behavior, so be sure to check it out.
