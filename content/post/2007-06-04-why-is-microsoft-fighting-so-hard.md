---
author: slowe
categories: Rant
comments: true
date: 2007-06-04T22:51:00Z
slug: why-is-microsoft-fighting-so-hard
tags:
- ESX
- Microsoft
- Virtualization
- VMware
title: Why is Microsoft Fighting so Hard?
url: /2007/06/04/why-is-microsoft-fighting-so-hard/
wordpress_id: 466
---

Think about it: Does it really matter to Microsoft if a server running Windows is physical or virtual? If a company buys ten servers running Windows Server, then Microsoft gets revenue for ten licenses of [Windows Server](http://www.microsoft.com/windowsserver/default.mspx). If a company buys two servers running [VMware ESX Server](http://www.vmware.com/products/vi/esx/) but has ten guests running Windows Server, Microsoft still gets license revenue. So what does it matter to Microsoft, as pointed out in [this article](http://searchwinit.techtarget.com/originalContent/0,289142,sid1_gci1256693,00.html):

>Customers will be using VMware or Virtual Iron, for example, and they will buy Windows and other Microsoft software to run on those virtual machines.

A VMware Virtual Infrastructure deployment does not preclude the use of Microsoft software; in fact, it often accelerates the deployment of more instances of Windows. How many times have you read about "virtual server sprawl"? I would wager that the majority of those sprawling virtual servers are still running Microsoft Windows. So why is Microsoft fighting so hard in the virtualization space?

Why try to create your own hypervisor to compete with VMware's wildly successful hypervisor in ESX Server? Why not partner with VMware to make your products _even better_ when running on VMware's virtualization layer? Why not build a management console that can manage VMware's virtualization technologies better than their own management console? I mean, if the real challenge is the management of VMs, why not tackle that?

Why not "embrace and extend"?
