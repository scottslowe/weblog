---
author: slowe
categories: Tutorial
comments: true
date: 2014-01-13T15:11:50Z
slug: running-synergy-on-os-x-mavericks
tags:
- CLI
- macOS
- UNIX
title: Running Synergy on OS X Mavericks
url: /2014/01/13/running-synergy-on-os-x-mavericks/
wordpress_id: 3396
---

In this post, I'm going to show you a workaround to running Synergy on OS X Mavericks. If you visit [the official Synergy page](http://ww.synergy-foss.org/), you'll note that the site indicates that full Mavericks support is still pending. However, if you're willing to "get your hands dirty," you can run Synergy on OS X Mavericks _right now._

If you're unfamiliar with Synergy, read this write-up (from 2 years ago) on [how I use Synergy in my home office setup][1]. The basic gist behind Synergy is that one computer will run the Synergy server; other computers will run the Synergy client and connect to the Synergy server. You'll be able to use the keyboard and mouse attached to the Synergy server to control the Synergy clients.

Here's how to get Synergy support running on OS X Mavericks now:

1. Download the latest 10.8 Synergy build from the website. (I didn't include a link here because the link changes as the version changes, so the link would become stale rather quickly.) This downloads as a .DMG file to your computer.

2. Double-click the .DMG to open and mount it on your desktop. Inside the .DMG, you'll see the Synergy app icon.

3. Right-click (or Ctrl-click) on the Synergy app and select "Show Package Contents."

4. Double-click on Contents, then MacOS.

5. In the MacOS file, copy the `synergys` and `synergyc` files to a different location. It doesn't really matter where, just make note of the location.

6. Close all the window and eject (unmount) the downloaded .DMG file.

For your Synergy server, you'll need an appropriate configuration file. You can check [my previously-mentioned Synergy post][1] for an example configuration file, or you can peruse [the official wiki](http://synergy-foss.org/wiki/User). Either way, create an appropriate configuration file, and make note of its name and location.

When you're ready, just launch the Synergy server from the OS X Terminal, like this (I'm assuming that `synergys` and its configuration file---creatively named `synergy.conf`---are stored in your home directory):

    ~/synergys -c ~/synergy.conf

Using whatever method you prefer, copy the previously-extracted `synergyc` file to your Synergy client(s). As before, it doesn't really matter too much where you put the file, just make a note of the location. Then, using the OS X Terminal, run this (as before, I'm assuming `synergyc` is in your home directory):

    ~/synergyc <Name of Synergy server>

That's it! You should now be able to use the keyboard and mouse on the Synergy server to control the Synergy client. I can verify that current builds of the Synergy client (`synergyc`) work just fine on OS X Mavericks, and I would imagine that the Synergy server would work fine as well (I just haven't had time to test it). If anyone has tested it and would like to provide feedback in the comments, I'm sure other readers would appreciate it.

Enjoy! (By the way, if you do find Synergy to be useful, I'd recommend donating to the project.)

[1]: {{< relref "2011-11-18-some-oss-with-my-mac-part-1-synergy.md" >}}
