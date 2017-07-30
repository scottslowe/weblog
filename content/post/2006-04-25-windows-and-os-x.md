---
author: slowe
categories: Musing
comments: false
date: 2006-04-25T11:54:25Z
slug: windows-and-os-x
tags:
- Apple
- IBM
- Macintosh
- Microsoft
- Virtualization
- Vista
- Windows
title: Windows and OS X
url: /2006/04/25/windows-and-os-x/
wordpress_id: 232
---

There are lots of industry pundits out there proclaiming that the introduction of [Boot Camp](http://www.apple.com/macosx/bootcamp/)---Apple's new beta application that simplifies and streamlines the installation of [Windows XP](http://www.microsoft.com/windowsxp/default.mspx) (and presumably [Windows Vista](http://www.microsoft.com/windowsvista/) as well) on Intel-based Macs---is simply the first step in a complex scheme that will eventually culminate in something much bigger. I'm not so sure about that.

The predictions range the whole gamut of possibilities. Some are predicting that [Apple](http://www.apple.com/) will begin reselling Windows XP pre-loaded on its Intel-based Macs, much in the way that MacMall is now doing. In this scenario, Apple differentiates itself from other x86 vendors in that it offers the only solution that will also run [Mac OS X](http://www.apple.com/macosx/). This is something that [Dell](http://www.dell.com/) won't be able to do.

Others are predicting that Apple will implement a Win32-compatible API, such as [Darwine](http://darwine.opendarwin.org/), so that Mac OS X will become a "better Windows than Windows" (quotes mine). Does anyone remember OS/2? Towards the end of OS/2's life, [IBM](http://www.ibm.com/) started positioning OS/2 as a better way to run Windows applications than Windows. Not too terribly long after that, OS/2 died. Will Mac OS X follow the same fate? No, there are too many differences between OS/2 and Mac OS X (and between IBM and Apple) to believe that these two technologically superior operating systems will take the same path. However, I do believe that it would be a mistake for Apple to try to position Mac OS X as a better way to run Windows applications than Windows itself. Microsoft's "embrace and extend" philosophy rarely works against them, and often backfires.

The most likely approach involves virtualization. Making it possible to run an instance of Windows under Mac OS X (using a hypervisor and built-in Intel VT technology), so that users can run those "legacy" Windows applications that don't have a native Mac equivalent. This makes the switch seamless and no risk. (Note further that Boot Camp additionally reduces the risk of switching, since a user can go back to Windows whenever needed).

I could be way off here; it certainly wouldn't be the first time. What do you think?
