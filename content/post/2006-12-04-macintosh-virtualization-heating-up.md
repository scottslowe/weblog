---
author: slowe
categories: Musing
comments: true
date: 2006-12-04T09:35:19Z
slug: macintosh-virtualization-heating-up
tags:
- Fusion
- Macintosh
- Virtualization
- VMware
title: Macintosh Virtualization Heating Up
url: /2006/12/04/macintosh-virtualization-heating-up/
wordpress_id: 376
---

[Parallels Desktop for Mac](http://www.parallels.com/en/products/desktop/) took the prize as the first virtualization solution for Intel-based Macs (OK, the first _commercial_ solution). Even so, heavyweight VMware's entry into the Mac market caused many to say, "Well, there goes Parallels."

Not so fast. I'm a huge VMware fan and make no secret of it. But in perusing the list of features for [this latest beta](http://forum.parallels.com/thread5997.html), I see a couple of features that VMware ought to be worried about:

* The ability to boot from a Boot Camp partition (this allows you to use a single installation of Windows XP that can be booted via Boot Camp or booted virtually)

* "Coherency mode", in which Windows windows (sorry, not really sure how else to put it) sit side-by-side with [Mac OS X](http://www.apple.com/macosx/) windows. It reminds me of the rootless X11 display that ships with Mac OS X and allows X11 and Aqua applications to interact with each other.

In my eyes, coherency mode is _huge_. The ability to run Windows applications side-by-side with Mac OS X applications (and I suppose X11 applications) is phenomenal. Throw in (eventually) seamless window support from [Citrix](http://www.citrix.com/) for their native Mac client and you've got yet another type of application window that can be resized, minimized, or moved just like any other. [TSClientX](http://desktopecho.com/tsclientx/) has already added support for [SeamlessRDP](http://www.cendio.com/seamlessrdp) to make seamless windows available via RDP as well. Why not in our virtualization solutions, too?

Then you add solutions built on WINE, which are designed to emulate the Win32 API on a non-Windows operating system, and you've got lots of options for coexistence. The one thing in common about all these solutions? Simple: _They're all built on Mac OS X._ Only the Mac operating system offers the ability to mix-and-match operating system windows on a single desktop at the same time. (To me, that is really cool.)

What's really odd, too, is that there was talk at VMworld 2006 about customers wanting this functionality ("coherency mode" or native windows) added to VMware Fusion. I guess now VMware has another set of reasons to look into this feature: the need to keep pace with the competition.
