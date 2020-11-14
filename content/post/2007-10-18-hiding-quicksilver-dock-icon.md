---
author: slowe
categories: Explanation
comments: true
date: 2007-10-18T17:08:20Z
slug: hiding-quicksilver-dock-icon
tags:
- macOS
title: Hiding Quicksilver Dock Icon
url: /2007/10/18/hiding-quicksilver-dock-icon/
wordpress_id: 561
---

A new version (B52, build 3812) of [Quicksilver](http://quicksilver.blacktree.com/) was released in the last few days, and today I upgraded to the new version. As I have stated here more than a few times, Quicksilver is one of the most useful---if not _the_ most useful---utility I have installed on my MacBook Pro. Yes, it's that good.

In any case, I found that after upgrading Quicksilver, I couldn't hide the Dock icon. I know that I shouldn't have worried about it, but it bothered me. I want Quicksilver to run, but I don't want the Dock icon there. After much searching and testing, I had almost resigned myself to having the Quicksilver icon in the Dock when I finally came across an [posting on the Blacktree forums](http://blacktree.cocoaforge.com/forums/viewtopic.php?t=3260) that gave me an idea. I had seen this post before, but had dismissed it as not useful to me; this time, however, I took another look.

It appears that the ability to hide Quicksilver's Dock icon requires read-write permissions to the Quicksilver application itself. Since I run as a non-admin user for day-to-day operations, I did not have read-write permission to the /Applications folder where Quicksilver was located and therefore did not have the ability to hide the Dock icon. Supposedly, you should be able to edit the `Info.plist` file (found in /Applications/Quicksilver/Contents) and change the LSUIElement value to 0, but I tried this and it had no effect (no effect, that is, other than to change the value of the "Show the Dock icon" preference for Quicksilver but leave it greyed out and disabled).

After a series of other attempts, I finally granted myself read-write access to the application itself. Voila! That fixed it. I was now able to show and hide the Dock icon at will. What was really interesting, though, was that even after setting the application to hide the Dock icon, removing the read-write permission caused the Dock icon to reappear. Very strange!

So, if you upgrade Quicksilver and find that you can no longer hide the Dock icon, grant yourself read-write permissions to the application and that should fix the problem.

**UPDATE:** I find that the Quicksilver Dock icon still appears when I log in, requiring a quick series of checking the box, restarting, unchecking the box, and restarting again. I still have no idea _why_ this is the case. Anyone have any suggestions?

**UPDATE 2:** An update to Quicksilver (Beta 53, Build 3814) fixes the issue, as far as I can tell.
