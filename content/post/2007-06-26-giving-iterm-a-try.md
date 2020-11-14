---
author: slowe
categories: Musing
comments: true
date: 2007-06-26T14:21:00Z
slug: giving-iterm-a-try
tags:
- macOS
- CLI
title: Giving iTerm a Try
url: /2007/06/26/giving-iterm-a-try/
wordpress_id: 477
---

It invariably occurs that, regardless of whether I'm on a computer running [Microsoft Windows Server 2003](http://www.microsoft.com/windowsserver/default.mspx), some flavor of Linux or UNIX, or [Mac OS X](http://www.apple.com/macosx/), I open a command prompt at some point or another during my session. I can't really explain why; it's just how I work. I find it faster to type `ipconfig /all` to get the networking configuration on a Windows box than right-clicking on My Network Places, selecting Properties, right-clicking the network connection, selecting Properties---you get the idea. I suppose it dates from my MS-DOS days, where I didn't really have any other choice but to use the command line.

And yet, even in this day of highly functional graphical user interfaces, I still find myself using the command line to get stuff done. Since Mac OS X is my primary OS (regular readers know that I switched about four years ago and now carry a Core 2 Duo-based [MacBook Pro](http://www.apple.com/macbookpro/)), that meant that I spent a fair amount of time using Terminal.app, the built-in terminal program that ships with the Mac OS X operating system. Within the last couple of days, though, I decided to give [iTerm](http://iterm.sourceforge.net/), an open source Mac OS X terminal program, a try to see if it would work as a Terminal.app replacement.

Why? The easiest answer would be that I am a "new software junkie"; I'm constantly looking for applications that provide newer/better ways of doing things. (Yet, interestingly enough, I am resistant to change once I have a routine that works: I [didn't like VirtueDesktops][1] when I first started using it, but then it became a natural part of my workflow; I had a [hard time getting used to Quicksilver][2] but now rely heavily on it; and after being forced to lose virtual desktops due to Tiger incompatibilities and instead use Expose, I now find it hard to go back to virtual desktops.) I came across a few references to iTerm and its AppleScript support and Quicksilver module and thought, "Why not?" So I decided to give it a try.

After downloading and installing iTerm, the very first thing I did was turn off the blasted (er, brushed) metal interface. (Have I ever mentioned I don't like brushed metal interfaces?) I then spent a few minutes customizing iTerm to look like I wanted it (which was basically like Terminal.app), and then started using it. OK, it supports tabs, that's kind of cool, but I _very_ rarely use tabs (even tabbed Web browsing is very rare), so I'm not really sure that helps much. Again, having grown accustomed to having lots of windows open (lots of browser windows, Terminal windows, Finder windows, whatever), I'm just not really into the idea of tabbed interfaces, and I generally turn them off. But tabs are a big selling point for iTerm, and without a need for tabs, why bother to switch at all?

That was a good question, and I didn't (and still don't, to some extent) have an answer for it. iTerm seems just as fast as Terminal, they both can be configured to work similarly and look similarly. So why switch? The only reason I can find so far is the idea of iTerm's _bookmarks_, which seem to be similar in function to Terminal.app's `.term` files. The idea is that you can open a bookmark that automatically runs a specific command (like opening an SSH session to a server or initiating an FTP connection). Each of these bookmarks could run in a new tab (by default) or in a new window (although I haven't figured out how to make it open in a new window by default---anyone out there have any suggestions?). OK, that's kind of handy, and I suppose I could get used to that, but the UI for managing bookmarks is absolutely atrocious. Here's hoping that portion of the program gets some attention from the developers soon.

&lt;aside&gt;Please don't mistake my dislike for certain portions of the iTerm interface for a lack of appreciation for the developers' hard work in creating iTerm.&lt;/aside&gt;

So, aside from the bookmarks feature, I haven't yet really found that one _outstanding_ reason to replace Terminal.app with iTerm. I can tell you what would make switching a no-brainer, though, in order of preference:

1. Ability to set bookmarks to open in a new window by default (can use Option key to reverse the behavior, like now)

2. Ability to access iTerm bookmarks via Quicksilver (the current Quicksilver module does not appear to have that functionality)

3. Spotlight support for iTerm bookmarks

Having these features incorporated into iTerm would make switching to iTerm from Terminal.app a foregone conclusion.

What about other "iTerm switchers" out there? Any suggestions or tips for me, or things I might be missing? I'd love to hear from you.

[1]: {{< relref "2005-09-30-virtual-desktops-on-mac-os-x.md" >}}
[2]: {{< relref "2006-07-16-trying-quicksilver.md" >}}
