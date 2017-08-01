---
author: slowe
categories: Information
comments: false
date: 2006-03-12T22:09:03Z
slug: my-tiger-upgrade
tags:
- Macintosh
title: My Tiger Upgrade
url: /2006/03/12/my-tiger-upgrade/
wordpress_id: 198
---

I've posted a couple of other entries about stuff related to the Tiger upgrade (like my discovery of [continued development of the Virtue virtual desktop application][1] and [weirdness with Spotlight][2]), but hadn't yet actually discussed the upgrade itself, and everything that was involved. So here's the sordid details.

When I upgraded from Jaguar to Panther, I performed an "Archive and Install" to get as clean a system as possible without having to reinstall all my applications and such (most of which, at the time, had come bundled with the PowerBook when I bought it). That worked well enough, but this time around I had some other changes I wanted to make, so I felt like an "Erase and Install" was the best approach.

So, not without some trepidation, I proceeded to backup all my data---mail messages, documents, bookmarks, music, pictures, etc.---to an external Firewire hard drive. Then, paranoid creature that I am, I made a manual copy of a few of the items as well (just in case). I double-checked and triple-checked to make sure I had gotten all the stuff I needed. I verified that I had licenses for software I had purchased. Finally, once I was reasonably satisfied that I had all the backups I needed, I inserted the Tiger DVD and rebooted the laptop.

The installation of Tiger was pretty painless, as it wasn't until after I had it running and had started installing applications that I started running into odd things:

* RDC Menu: This freeware app lets me run multiple instances of Microsoft's Remote Desktop Connection for the Mac---a feat that Windows users take for granted. Under Panther, the Dock icon for this application had worked perfectly; under Tiger, it doesn't work at all. I finally managed to get it working from the menu bar, but I'm not terribly thrilled with it there.

* SSL Enabler: This GUI front-end to the [Stunnel](http://stunnel.mirt.net/) utility won't work unless you first create the `/usr/local/sbin` directory. Otherwise, the Stunnel program doesn't get copied correctly and nothing works.

So far, it's only been these few issues, along with the Spotlight problem described separately, and everything else has been working reasonably well.

Of course, I did have to reinstall all my applications, recreate some of my preferences, and such, but I prefer that knowing that I have a clean installation of Tiger that is unencumbered by any problems caused from my Panther installation.

Overall, I'm glad I upgraded. I do find myself liking Spotlight, and using Spotlight, and I'm looking forward to working more with Automator. This will also now open the door to test some additional applications that require Tiger or higher in order to run.

[1]: {{< relref "2006-03-11-a-new-life-for-virtue.md" >}}
[2]: {{< relref "2006-03-11-spotlight-weirdness.md" >}}
