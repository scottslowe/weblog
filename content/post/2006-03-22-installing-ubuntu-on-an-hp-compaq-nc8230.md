---
author: slowe
categories: Information
comments: false
date: 2006-03-22T16:59:20Z
slug: installing-ubuntu-on-an-hp-compaq-nc8230
tags:
- HP
- Linux
- Ubuntu
title: Installing Ubuntu on an HP Compaq nc8230
url: /2006/03/22/installing-ubuntu-on-an-hp-compaq-nc8230/
wordpress_id: 207
---

I was issued a new [HP Compaq nc8230](http://h10010.www1.hp.com/wwpc/us/en/sm/WF06b/321957-64295-89315-321838-f1-447338-447352-451419.html?jumpid=oc_R1002_USENC-001_HP%20Compaq%20nc8230%20Notebook%20PC&lang=en&cc=us) laptop today, with the standard corporate image of Windows XP Professional Service Pack 2 and the assorted applications. One of the very first things I did was install [Ubuntu](http://www.ubuntu.com/) 5.10. Here's some additional information on a few of the hurdles involved (there aren't many, fortunately).

I was already familiar with Ubuntu 5.10, having already installed it for my daughters on two older Compaq laptops. However, I'd never installed it on brand-new equipment, so I was a bit concerned that all the hardware wouldn't be detected properly. A bit of searching came up with this article indicating that [Ubuntu 5.04 had installed succesfully](http://www.freedesktop.org/~jg/nc8230), so I felt fairly confident that everything would be fine.

So I popped in the Ubuntu installation CD, pressed Enter when prompted, and was soon greeted with a blank screen. That was odd. I rebooted, and got the same behavior. Upon the next reboot, I pressed F1 at the "boot:" prompt to review some parameters. I quickly stumbled onto the "vga=771" parameter. At the boot prompt, I used "linux vga=771" and the system booted into the installation menu. My first hurdle was overcome.

The rest of the installation seemed to go smoothly, right up until the point where the installation crashed with a message that it couldn't copy files from the CD-ROM. In fact, it couldn't detect that the CD-ROM drive even existed.

I rebooted, tried again, got a little bit farther, and got the same message again. Examining the CD a bit closer, I didn't see anything wrong with the disc (no obvious scratches, dirt, smudges, etc.), but cleaned the CD nevertheless and tried again. This time the installation process was successful, and everything was golden.

Until the X Server didn't work on the final reboot. Sigh. Referring back to [this article I'd found earlier](http://fsiu.uwc.ac.za/kinky/index.php?module=wiki&action=wikilink&pagename=Howto+for+Ubuntu+Breezy+on+the+HP+Compaq+nc8230+la), I followed the instructions to remove (if possible) and reinstall the xorg-driver-fglrx package and then reconfigure X. When I had finally completed those steps, the X Window System started up and dropped me onto a customized GNOME desktop. Finally!

From there, I proceeded to install a 686 kernel (instead of the generic 386 kernel) and run an `apt-get upgrade` to pick up the latest packages from the Ubuntu repositories. So far (knock on wood), everything has been pretty stable and pretty functional.

**UPDATE:** Scratch that functional part. The laptop has locked up more times in _one afternoon_ than my PowerBook has in the 2+ years that I've owned it. I'm not really sure what's going on with this thing, but clearly something isn't quite right.
