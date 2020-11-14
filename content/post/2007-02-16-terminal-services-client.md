---
author: slowe
categories: Review
comments: true
date: 2007-02-16T12:13:30Z
slug: terminal-services-client
tags:
- macOS
- Microsoft
- Windows
title: Terminal Services Client
url: /2007/02/16/terminal-services-client/
wordpress_id: 414
---

[TSclientX](http://desktopecho.com/tsclientx/) is (according to the website) "an alternative RDP Client for Mac OS X". I'd describe it as more than that, but I suppose that will suffice. I actually had the opportunity to meet Daniel Milisic, TSclientX's maintainer, in Los Angeles at VMworld 2006.

Although I'd used TSclientX off and on for a while, it wasn't until just recently that I started using TSclientX full-time as my primary RDP client. Until that time, I'd been using the "official" [RDP client from Microsoft](http://www.microsoft.com/mac/downloads.aspx?pid=download&location=/mac/download/misc/rdc_update_103.xml&secid=80&ssid=10&flgnosysreq=True), even though it is still PowerPC only (no Intel or Universal Binary available yet). To get around the limitation of only one instance of the Microsoft RDP client running at a time (a serious limitation for me, since I often have multiple connections open at any given time), I was using [RDC Menu](http://www.xutils.com/rdcmenu/). It was a bit strange in that RDC Menu was a Universal Binary, although the RDP Client was not; it still worked fine, though.

So why did I switch? Well, for a number of reasons:

1. TSclientX is faster. As a Universal Binary, it's both faster to launch (even including the time to launch X11, if it's not already running) and faster during operation. Not that the official client's performance was awful, but the Rosetta overhead is there. TSclientX doesn't have that Rosetta overhead.

2. TSclientX didn't interfere with [Quicksilver](http://quicksilver.blacktree.com/). Now that I'm a Quicksilver junkie, it irritated me that I couldn't invoke Quicksilver when I was inside an RDP session. I don't run into that problem with TSclientX, thankfully.

3. Using Apple's standard X11 environment, I can easily create a hotkey to launch TSclientX when X11 is active. While this won't replace Quicksilver, it can be handy at times nevertheless. The path to use in creating a hotkey in X11 would be "/Applications/TSclientX.app/Contents/Resources/launcher.sh" (be sure to replace "/Applications" with the location of where you installed TSclientX, but I think that it doesn't run properly anywhere else).

The one drawback I have found so far is that all instances of TSclientX share the X11 icon in the dock, so it makes it a tad more difficult to switch between multiple sessions using Cmd-Tab or the dock. This isn't a huge deal, since Expose works properly (I use F9 most of the time), but it does take a bit of adjustment to get used to that.

It would be great if someone wrote a native Aqua (not X11) version of this application, but until then TSClientX fits the bill quite nicely. If you haven't tried it, go give it a swing.
