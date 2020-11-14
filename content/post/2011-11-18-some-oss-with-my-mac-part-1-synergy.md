---
author: slowe
categories: Explanation
comments: true
date: 2011-11-18T16:38:19Z
slug: some-oss-with-my-mac-part-1-synergy
tags:
- Linux
- macOS
- OSS
title: 'Some OSS with my Mac, Part 1: Synergy'
url: /2011/11/18/some-oss-with-my-mac-part-1-synergy/
wordpress_id: 2468
---

(This is Part 1 of a two-part series on some open source software I've incorporated into my home office setup, which is primarily Mac-based. Part 2 is [here][1].)

Since my move to Denver a couple months ago, I've had a home office large enough to do a full multi-computer setup. Space constraints in my home office in Raleigh-Durham had prevented me from being able to fully take advantage of the multiple laptops that I have. So, with the new space in Denver, I set out to go full-bore. At the time, I had a late 2006 era 15" MacBook Pro, a mid-2009 MacBook Pro 15", and a Dell Latitude E6400 running Ubuntu Linux. (I've since replaced the late 2006 15" MacBook Pro with an early 2011 13" MacBook Pro.)

Of course, the real challenge with a multi-computer setup is the need for a shared keyboard and mouse, and that was the first challenge I had to tackle. The answer was found in [Synergy](http://synergy-foss.org/), an open source project that provides a shared keyboard and mouse across multiple computers.

For those unfamiliar with Synergy, here's a little background: one computer (known as the "server") hosts the keyboard and mouse. The server's keyboard and mouse are shared with other computers referred to as "clients". The server/client designation makes sense to me now, but I recall at the time it seemed a bit odd.

I messed around with a few different distributions or variations of Synergy (like [SynergyKM](http://sourceforge.net/projects/synergykm/)) for a little while before realizing that only the "real" version of Synergy was going to give me the flexibility that I wanted. For example, I wanted a bit more control over how and when the cursor jumped from screen to screen, and none of the prepackaged versions of Synergy offered customizable controls, even though the underlying Synergy package did.

From [this Synergy downloads page](http://synergy-foss.org/download/), I downloaded the Mac and Linux versions of the 1.4.x beta version of Synergy. Despite my best efforts, I could _not_ make it work. Keystroke mappings were off, the screen switching was intermittent, and it was buggy. Finally, I dropped back to the latest stable release at the time (1.3.6). Using the exact same configuration files, the 1.3.6 release worked _flawlessly_ the first time. Nice!

Here's my setup:

* One of the MacBook Pro laptops (the 15" 2009 MBP originally, now the 13" 2011 MBP) is the Synergy server. It sits in the "center" of the three computers on my desk. It has a 24" Apple Cinema display, external keyboard, Bluetooth Magic Mouse, and Magic Trackpad.

* The other MBP (formerly the 15" 2006 MBP, now the 15" 2009 MBP) sits to the left of the Synergy server. It is, of course, a Synergy client.

* The Dell laptop running Ubuntu Linux sits to the right of the Synergy server and is a Synergy client.

With that in mind, here is a sanitized version of the `synergy.conf` file I use on the Synergy server (currently the 13" 2011 MacBook Pro):

```text
section: screens
    center-laptop:
    left-laptop:
    right-laptop:
end

section: aliases
    center-laptop:
        center-laptop.domain.com
        center-laptop-wlan
        center-laptop-wlan.domain.com
    right-laptop:
        right-laptop.domain.com
        right-laptop-wlan
        right-laptop-wlan.domain.com
    left-laptop:
        left-laptop.domain.com
        left-laptop-wlan
        left-laptop-wlan.domain.com
end

section: links
    center-laptop:
        left = left-laptop
        right = right-laptop
    right-laptop:
        left = center-laptop
    left-laptop:
        right = center-laptop
end

section: options
    screenSaverSync = false
    switchCorners = bottom-right
    switchCornerSize = 20
    switchDoubleTap = 150
    keystroke(control+left) = switchInDirection(left)
    keystroke(control+right) = switchInDirection(right)
end
```

The [documentation for Synergy](http://synergy-foss.org/tracker/projects/synergy/wiki/Docs) is pretty straightforward, so I'd recommend you read the docs for more details on each of the directives and configuration options above and their function. What this configuration gives me:

* I can use Control+Left Arrow to move one screen to the left, and Control+Right Arrow to move one screen to the right.

* The `switchDoubleTap` option allows me to have to "double-tap" the edge of the screen before it will switch to the next screen; this prevents me from accidentally switching screens when I didn't mean to.

* The Mac versions of Synergy didn't support synchronizing the screen saver, hence the `screenSaverSync = false` statement.

* Although it's not explicitly called out anywhere here, installing the stable 1.3.6 build of Synergy on my Ubuntu laptop was straightforward, and launching Synergy is as simple as `/path/to/synergyc center-laptop`.

On the Macs, launching Synergy (as either server or client) is handled via shell script called by [ControlPlane](http://controlplane.dustinrue.com/), a handy Mac application that is a fork of the older (and apparently abandoned) Marco Polo project. The shell script is quite simple; it checks to see if Synergy is running and launches it if it isn't. (Nothing special there.) On the Linux laptop, I launch it manually as needed.

If you use multiple computers in your office, I'd **strongly** recommend having a look at Synergy---I've found it to be quite useful. If anyone has any other tips or tricks pertaining to Synergy or any of the other topics mentioned in this post, please feel free to speak up in the comments.

[1]: {{< relref "2011-11-21-some-oss-with-my-mac-part-2-unison.md" >}}
