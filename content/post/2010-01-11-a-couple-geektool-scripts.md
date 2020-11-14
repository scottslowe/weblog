---
author: slowe
categories: Explanation
comments: true
date: 2010-01-11T12:30:09Z
slug: a-couple-geektool-scripts
tags:
- CLI
- macOS
- UNIX
title: A Couple GeekTool Scripts
url: /2010/01/11/a-couple-geektool-scripts/
wordpress_id: 1794
---

I've been experimenting with [GeekTool](http://projects.tynsoe.org/en/geektool/), a nifty Mac OS X Preference Pane that allows you to display information on your desktop. This information can be static text, images, or the output of a script. The last option is the most useful one, in my opinion, and that's where I've been putting GeekTool to use for me. This isn't going to be some long post on how to use GeekTool or why you should install it; rather, I just wanted to share a couple of short scripts that I wrote that you might find useful.

I use Mac OS X's network location support extensively. I have separate locations for home (where I have a proxy server) and when I'm out and about (where there generally is no proxy server). So it's important for me to be able to tell, quickly, which location is active. If the wrong location is active, then network connectivity is impaired.

To help, I use this command with GeekTool to display the network location on my desktop:

	echo "Location: `networksetup -getcurrentlocation 2>&1 | tail -n 1`"

Note that if you are running as an administrative user on your Mac (which I don't in order to reduce potential security risks), then the `networksetup` command I use above will probably behave differently for you. Since I'm not running as an administrative user, `networksetup` would throw an error at the command line. Thus why I had to redirect STDERR to STDOUT and filter it using `tail`. Now, a quick F11 to show the desktop and I can immediately see which network location is active.

I also recently added a script to show me what proxy servers are currently active. This is in anticipation of starting my new job at EMC. I don't know if they have proxy servers on their network, but in the event they do I thought this next command might be handy:

	echo "HTTP Proxy: `scutil --proxy | grep HTTP | sort | sed -n '3,3p' | awk '{print $3}'`"

This command displays the HTTP proxy host configured in your network settings. So, again, a quick F11 allows me to see which proxy hosts are configured and active on my Mac.

I actually wrapped several of these commands together into a shell script that you can download [here][1] if you'd like. I'm sure there is probably some bash black magic that could produce this output in a more efficient way; feel free to post suggestions for improvement if you have any!

Of course, I also have a few other scripts running with GeekTool---one that displays system information, one that produces IP addresses and Airport (wireless LAN) information, etc.---but these two are probably most useful to me so far.

[1]: /public/dl/proxyinfo.zip
