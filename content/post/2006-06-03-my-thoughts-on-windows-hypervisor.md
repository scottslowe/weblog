---
author: slowe
categories: Musing
comments: false
date: 2006-06-03T16:05:38Z
slug: my-thoughts-on-windows-hypervisor
tags:
- Microsoft
- Virtualization
- Windows
title: My Thoughts on Windows Hypervisor
url: /2006/06/03/my-thoughts-on-windows-hypervisor/
wordpress_id: 259
---

A [recent posting on virtualization.info](http://www.virtualization.info/2006/05/microsoft-shows-windows-hypervisor.html) discussed a presentation at WinHEC 2006 regarding the Windows Hypervisor and its (projected) capabilities. Here are my thoughts.

First, and perhaps foremost, Windows Hypervisor (code-named "Viridian") is at least 3 years away. Based on the above article and [others](http://www.virtualization.info/2006/04/is-microsoft-also-approaching.html), Windows Hypervisor is slated for released in the Longhorn R2 time frame ("Longhorn R2" would be the minor release scheduled for two years after the release of Longhorn Server). That makes any discussion of Windows Hypervisor, its capabilities, its pricing and placement, etc., nothing more than vaporware---classic [Microsoft](http://www.microsoft.com/) vaporware.

Microsoft's demonstrations of Windows Hypervisor seem to be like their early demonstrations of [Windows Vista](http://www.microsoft.com/windowsvista/), which then got put on the chopping block and had most of the major features Microsoft had touted so heavily taken out. WinFS? Sorry, that's not going to make it into Vista. Live modification of virtual hardware? Sorry, that's not going to make it into Windows Hypervisor.

I'm sure that Microsoft will eventually deliver a hypervisor-based version of Windows, and that significant portions of the functionality that now requires [Xen](http://www.xensource.com/xen/xen/) or [VMware](http://www.vmware.com/) will be handled by the hypervisor. I'm also reasonably sure that the product will be late and most likely will not have all the features Microsoft says it will have. Finally, I'm equally sure during the two to three years that Microsoft will be baking the Windows Hypervisor that vendors such as VMware and projects such as Xen aren't going to stand still; they will continue to innovate and drive the future of server virtualization.
