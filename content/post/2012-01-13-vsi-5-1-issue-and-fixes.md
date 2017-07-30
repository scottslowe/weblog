---
author: slowe
categories: Information
comments: true
date: 2012-01-13T12:56:06Z
slug: vsi-5-1-issue-and-fixes
tags:
- EMC
- Storage
- Virtualization
- VMware
title: VSI 5.1 Issue and Fixes
url: /2012/01/13/vsi-5-1-issue-and-fixes/
wordpress_id: 2516
---

As you might have already seen (Chad broke the news [here](http://virtualgeek.typepad.com/virtual_geek/2012/01/emc-virtual-storage-integrator-v51released.html), Itzik has more details [here](http://itzikr.wordpress.com/2012/01/10/virtual-storage-integrator-5-1-is-here/)), EMC has released version 5.1 of the Virtual Storage Integrator (VSI), the vCenter plugin available to customers at no additional charge. I won't revisit all the new features; rather, I wanted to bring to your attention a late-discovered issue with the VSI 5.1 release. If this information hasn't already made it into the Release Notes, it should be there soon (there's also an EMC Primus case being created).

Here's the lowdown: there's an issue with VSI Storage Viewer 5.1 that prevents it from properly upgrading previous versions of VSI. After an upgrade to Storage Viewer 5.1, the user may encounter an error like "Unable to load DLL 'SEWrapper.dll': The specified module could not be found" (or similar).

The problem isn't in SEWrapper.dll, which is working properly and is, in fact, installed on the system. The problem is in the Microsoft Visual C++ 2010 Redistributable Package (x86) isn't installed, and that's a pre-requisite for the SEWrapper.dll library.

There are two workarounds for this issue:

1. Remove VSI Storage Viewer 5.1 and re-install it from scratch. This will cause the Visual C++ redistributables to get installed correctly, fixing the problem. **Note:** If you have older VSI features installed---like Storage Pool Management 5.0 or Path Management 5.0--reinstall those _after_ Storage Viewer 5.1 is installed.

2. Download and install the Microsoft Visual C++ 2010 Redistributable Package (x86) separately. You should be able to download that [here](http://www.microsoft.com/download/en/details.aspx?id=5555).

This will be in the Release Notes very soon (if not already), but I wanted to highlight the problem---and the fixes---here just in case.
