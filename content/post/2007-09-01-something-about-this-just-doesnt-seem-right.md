---
author: slowe
categories: Rant
comments: true
date: 2007-09-01T14:43:15Z
slug: something-about-this-just-doesnt-seem-right
tags:
- Microsoft
- Networking
- Vista
- Windows
title: Something About This Just Doesn't Seem Right
url: /2007/09/01/something-about-this-just-doesnt-seem-right/
wordpress_id: 517
---

First, let's set the stage. I first became aware of this issue---network slowdowns when playing multimedia files in Microsoft Windows Vista---when [this article by Adrian Kingsley-Hughes at ZDNet](http://blogs.zdnet.com/hardware/?p=726) popped up in one of my NetNewsWire subscriptions. I read the article and found that I was apparently coming in on the middle of an ongoing story, so I read [Mark Russinovich's response to the slowdowns with Vista](http://blogs.technet.com/markrussinovich/archive/2007/08/27/1833290.aspx). As usual, Mark's work was concise, clear, very informative, and well-written. Seeking more information, I then turned to [an article by Larry Osterman](http://blogs.msdn.com/larryosterman/archive/2007/08/28/windows-vista-sound-causes-network-throughput-slowdowns.aspx) (apparently a Microsoft multimedia developer) with more details on the issue.

Basically, the issue is this: when playing a multimedia file under [Windows Vista](http://www.microsoft.com/windowsvista/), network slowdown is intentionally throttled so as to allow the multimedia subsystem to have priority on the CPU, thus preventing any interference with the multimedia playback. Granted, this is a gross simplification of the various issues; I encourage you to read Mark's blog entry for complete and concise details.

Something about this just doesn't seem right. So you're telling me that this next-generation operating system that took thousands of man-years of developer time and many millions of dollars to create has to throttle network traffic in order to ensure smooth multimedia playback? You've got to be kidding me.

Now, before I could just assume that this is a Vista flaw (which I believe that it is), I felt like I had to do a little bit of additional research. It didn't take long to come across [this article](http://blog.rlove.org/2007/08/those-dang-dpcs-clogging-mmcss.html) by Robert Love (author of _Linux System Programming_ by O'Reilly and _Linux Kernel Development_ by Novell Press) which makes it pretty clear that Linux doesn't suffer from this sort of problem. Any [Mac OS X](http://www.apple.com/macosx/) programmers care to verify for us if there are similar issues in OS X?

With stuff like this in Vista (but not in previous versions of Windows, such as [Windows XP](http://www.microsoft.com/windowsxp/)), is it any wonder why people are so hard on Microsoft?

Oh, yes, one other thing---if you're already running Vista and being affected by this issue, check out [this fix](http://courtneymalone.com/2007/08/28/a-note-on-vista-network-speed/).
