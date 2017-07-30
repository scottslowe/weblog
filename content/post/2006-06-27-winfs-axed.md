---
author: slowe
categories: News
comments: false
date: 2006-06-27T09:30:47Z
slug: winfs-axed
tags:
- Microsoft
- Storage
- Windows
title: WinFS Axed
url: /2006/06/27/winfs-axed/
wordpress_id: 281
---

The news is all over the 'Net: [Microsoft](http://www.microsoft.com/) has killed WinFS as an independent product and will instead roll the technology into the next version of [SQL Server](http://www.microsoft.com/sqlserver/), code-named "Katmai."

I hate to say this, but I'm not really surprised. Microsoft has been talking about this abstract storage strategy since the pre-NT4 days, when "Cairo" was going to introduce the Object File System. The idea keeps getting pushed farther, and farther, and farther back...

In the meantime, technologies and products such as [Spotlight](http://www.apple.com/macosx/features/spotlight/) (for [Mac OS X](http://www.apple.com/macosx/)), [Beagle](http://beagle-project.org/Main_Page) (for Linux), and [Google Desktop](http://desktop.google.com/) (for Windows) continue to improve and continue to make this kind of sweeping technological change irrelevant. Is that why Microsoft killed WinFS? Do they feel the need for WinFS has been eliminated due to pervasive search technologies being embedded in the OS? Perhaps, but I would disagree with that belief. Tacking search technologies onto a file system instead of fixing the file system is just covering up the problem.

Instead, why not focus on more fully utilizing the functionality of the file system you've got? Why is Windows still relying on file name extensions to determine file types when that information could be stored in an alternate data stream directly in NTFS? Why aren't applications being written to be able to take advantage of things like alternate data streams, or extensible properties?
