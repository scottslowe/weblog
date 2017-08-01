---
author: slowe
categories: Rant
comments: true
date: 2008-02-09T22:32:51Z
slug: im-glad-im-not-the-only-one
tags:
- ESX
- Virtualization
- VMware
title: I'm Glad I'm Not The Only One
url: /2008/02/09/im-glad-im-not-the-only-one/
wordpress_id: 629
---

It would appear that Bob Plankers of [LoneSysAdmin](http://lonesysadmin.net/) and I are thinking along the same lines. Shortly after I posted a [brief notice][1] of the Storage VMotion GUI, Bob posted a [rant on the overall quality](http://lonesysadmin.net/2008/02/09/storage-vmotion-gui-stepping-backwards/) of VirtualCenter 2.5. In Bob's words, I don't even have to post, because he said exactly what I was going to say.

I didn't hesitate to proclaim my [disappointment in the Remote CLI][2], but I had hesitated to post my disappointments regarding other aspects of version 2.5 of VirtualCenter. It's slower than earlier versions on the same hardware, and overall it feels...unfinished. Almost like VMware rushed to get this out the door too quickly, perhaps as if they were feeling the pressure from Citrix/XenSource, Microsoft, and Virtual Iron and felt like they _absolutely must_ get this software out before it's completely done. After reading Bob's post, I'm glad to see I'm not the only one feeling this way.

It's all through the product. VMware introduces support for jumbo frames, but only for virtual machines---not for the VMkernel to use with iSCSI or NFS datastores, where it could really help. VMware gives us Storage VMotion, but hobbles it with the Remote CLI.

C'mon, VMware, there's no need to rush things. Organizations use your products because they are rock-solid and they provide the essential functionality required. Focus on the solidity. Capitalize on existing strengths. Shore up weakness, but don't introduce new weaknesses during the process. Only then will you be able to fend off Microsoft's "good enough" hypervisor bundled with every copy of Windows Server 2008 Enterprise.

So it's clear that Bob and I aren't too terribly happy with the overall fit and finish of VMware Infrastructure 3 version 3.5. It's a good product, yes, but it could have been a _great_ product. Now it looks like we'll need to wait until version 3.5.1 until we can expect that.

What about you? What do you think?

[1]: {{< relref "2008-02-09-graphical-front-end-for-storage-vmotion.md" >}}
[2]: {{< relref "2008-01-07-underwhelmed-by-the-remote-cli.md" >}}
