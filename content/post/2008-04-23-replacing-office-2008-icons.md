---
author: slowe
categories: Tutorial
comments: true
date: 2008-04-23T21:09:28Z
slug: replacing-office-2008-icons
tags:
- macOS
- Microsoft
- Office
title: Replacing Office 2008 Icons
url: /2008/04/23/replacing-office-2008-icons/
wordpress_id: 691
---

In the last few days, I upgraded my work laptop to Office 2008, the latest version of Microsoft's office suite for Mac OS X. I was looking forward to the Universal binaries (no more sluggish performance due to Rosetta) and Office 2007 file format compatibility. I did get those, but what I also got were some really ugly icons.

So, I did what any enterprising Mac user would do. I replaced them with a new set.

Here's what the new icons look like:

![New Office 2008 icons](/public/img/new-office-icons.jpg)

I found the replacement icons [here](http://www.deskmodr.com/?s=Office); look for the set titled "HLD V2 by Henri Liriani". The icons are well designed and come in a variety of sizes. Upon unpacking the downloaded file, I simply took the .ICNS files in the package and replaced the original files in the application bundles.

For example, right-clicking on Microsoft Entourage and selecting "Show Package Contents," opening the Contents folder, then opening the Resources folder will show where the new icon needs to be copied. The original icon is in a file named `Entourage.icns`. Rename this file (just in case you need the old one) and replace it with the downloaded version, making sure that you name the downloaded version with the same original name. Poof---Entourage now has a new icon.

For the other applications, the files to replace are:

	Microsoft Word/Contents/Resources/MSWD.icns  
	Microsoft Excel/Contents/Resources/XCEL.icns  
	Microsoft PowerPoint/Contents/Resources/PPT3.icns

Replace each of these files, keeping the filename intact, with the new versions.

Once that's done, you too can enjoy much more pleasant Office 2008 icons!
