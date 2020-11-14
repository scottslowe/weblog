---
author: slowe
categories: Review
comments: true
date: 2011-06-17T09:00:00Z
slug: trying-iterm2
tags:
- CLI
- macOS
- UNIX
title: Trying iTerm2
url: /2011/06/17/trying-iterm2/
wordpress_id: 2316
---

In 2007 I posted [an article about iTerm][1], an open source terminal application for Mac OS X. At that time, I gave up on iTerm and switched back to the Apple-supplied Terminal.app. For a while, it seemed as if iTerm was stagnating and development had stalled.

However, I recently learned that some developers forked the original iTerm to create [iTerm2](http://www.iterm2.com/). Like the original iTerm, iTerm2 supports AppleScript and tabbed terminals. The tabbed terminals I don't really care about (I don't use tabs, generally speaking), but I do like AppleScript support (in case you hadn't picked that up already). There are also some other interesting features: split panes, a Visor-like window accessible via hotkey, and [Growl](http://growl.info/) support.

So I installed the latest build of iTerm2, and so far it's been very stable. My only complaint has been that you can't configure iTerm2 to spawn new windows instead of new tabs by default. Key point: I started using [the Remote Hosts plug-in for Quicksilver](https://github.com/skurfer/RemoteHosts-qsplugin) (great plug-in, by the way). Once I reconfigured iTerm2 as the handler for `ssh://` URIs, the plug-in stopped spawning Terminal.app windows and starting spawning iTerm2 tabs instead. Unfortunately, I haven't been able to figure out how to tell it to spawn a new iTerm2 window instead. (Feel free to chime in with ideas.)

I also whipped up a quick AppleScript that I can invoke with [FastScripts](http://www.red-sweater.com/fastscripts/); the purpose of this script is to open a new iTerm2 terminal window at the same directory as the frontmost Finder window.

I'm going to continue to work with iTerm2 as my primary terminal application for a while to see how it works. If anyone has any tips or tricks to share, please add them in the comments below. Thanks!

[1]: {{< relref "2007-06-26-giving-iterm-a-try.md" >}}
