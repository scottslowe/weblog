---
author: slowe
categories: Information
comments: true
date: 2017-03-06T00:00:00Z
tags:
- Linux
- Windows
- CentOS
- Fedora
title: 'The Linux Migration: Other Users'' Stories, Part 2'
url: /2017/03/06/linux-migration-other-users-stories-pt2/
---

This post is part of a series of posts sharing the stories of other users who have decided to migrate to Linux as their primary desktop OS. Each person's migration (and their accompanying story) is unique; some people have embraced Linux only on their home computer; others are using it at work as well. I believe that sharing this information will help readers who may be considering a migration of their own, and who have questions about whether this is right for them and their particular needs.

For more information about other migrations, see [part 1][xref-1] or part 2 of the series.

This time around we're sharing the story of Rynardt Spies.

**Q: Why did you switch to Linux?**

In short, I've always been at least a part-time Linux desktop user and a heavy RHEL server user. My main work machine is Windows. However, because of my work with AWS, Docker, etc., I find that being on a Linux machine with all the Linux tools at hand (especially OpenSSL and simple built-in tools like SSH) is invaluable when working in a Linux world. However, I've always used [Linux Mint][link-1], or [Ubuntu][link-2] (basically [Debian][link-4]-derived distributions) for my desktop work, and RHEL/[CentOS][link-3] for server workloads.

**Q: Which distribution of Linux did you choose?**

At first, I tried a [Fedora 25 Cinnamon][link-5] desktop. I come from a RHEL background, so could never really warm to Ubuntu. Now that desktop ports on RH Fedora seems close to be on par with Ubuntu/Mint, I think the switch back to RHEL from Ubuntu is possible for desktop, at least for me. After a few weeks, I switched from Cinnamon back to [GNOME 3][link-6], and that's where I am currently.

**Q: What sort of hardware are you using?**

I'm using a Lenovo X201 (an old laptop I've had laying around for a while). I've upgraded the memory to 8GB RAM, and replaced the old spindle with a Samsung 850 PRO 256GB SSD for this Linux trial. I'll see where this goes from a functionality point of view before spending money on new hardware.

So far, I've got just about everything working that I need.

**Q: What applications are you using on Linux?**

Some of the tools/applications that I regularly use include:

* Dropbox
* Google Chrome (mainly for Postman, Tweetdeck, and other Chrome-based applications or extensions)
* Firefox
* Cryptkeeper as a front-end to EncFS (which I use to encrypt/decrypt Dropbox folders, using BoxCryptor on Windows/Android)
* KeePass 2.x (I've been using this for years for password management and have it set up well across multiple systems)
* Skype client (although the Skype web app seems to be better)
* LibreOffice
* Google Docs
* Cisco AnyConnect VPN client
* Evolution (for mail and calendar)
* Git (of course)
* Arduino IDE (for some electronics projects I enjoy)
* Docker
* Inkscape, MyPaint, Shutter, Krita
* KRDC for RDP access (still trying to find a better solution, but this works)
* Sublime Text 3

I also play with some of the other tools on the system from time to time, but they aren't all worth mentioning.

**Q: Anything else you'd like others to know?**

Although most of the applications run well and the workflow for using the distribution is effortless, there are a few niggles that come up from time to time. However, none of them are show stoppers. One of the issues I have is that the display driver crashes randomly, but those crashes are few and far between.

I started out on Fedora 25 using the Cinnamon desktop, a desktop system which I had used extensively in the past. In fact, I used Cinnamon with Linux Mint 13 for two years as my main work machine without any issues. At first all worked well on this new Fedora build. However, I soon started seeing issues with stability, and there were times when I couldn’t even log in due to the desktop system not responding at all. So in the end I made the decision to switch to GNOME. It’s definitely not as customizable as Cinnamon, but it does work well enough for me to want to stick with it for now.



[link-1]: https://linuxmint.com/
[link-2]: https://www.ubuntu.com/
[link-3]: https://centos.org/
[link-4]: https://www.debian.org/
[link-5]: https://spins.fedoraproject.org/en/cinnamon/
[link-6]: https://www.gnome.org/gnome-3/
[xref-1]: {{< relref "2017-03-01-linux-migration-other-users-stories-pt1.md" >}}
