---
author: slowe
categories: Information
comments: true
date: 2017-03-10T00:00:00Z
tags:
- Linux
- Productivity
- macOS
- Ubuntu
title: 'The Linux Migration: Other Users'' Stories, Part 3'
url: /2017/03/10/linux-migration-other-users-stories-pt3/
---

Over the last few weeks, I've been sharing various users' stories about their own personal migration to Linux. If you've not read them already, I encourage you to check out [part 1][xref-1] and [part 2][xref-2] of this multi-part series to get a feel for why folks are deciding to switch to Linux, the challenges they faced, and the benefits they've seen (so far). Obviously, Linux isn't the right fit for everyone, but at least by sharing these stories you'll get a better feel whether it's a right fit for you.

This is Brian Hall's story of switching to Linux.

**Q: Why did you switch to Linux?**

I've been an OS X user since 2010. It was a huge change coming from Windows, especially since the laptop I bought had the first SSD that I've had in my primary machine. I didn't think it could get any better. Over the years that feeling started to wear off.  

OS X started to feel bloated. It seemed like OS X started to get in my way more and more often. I ended up formatting and reinstalling OSX like I used to do with Windows (maybe not quite as often). Setting up Mail to work the way I wanted it to was a pain. I had apps for everything. That started out fun, but when it came to getting my data out of each of them it was a tough job.

I wanted this to change. It seemed like getting my personal machine set up the way I was using it was entirely too complex. If my machine were to die, it would take me weeks to get it back to the way it was. I wanted to simplify as much as possible.

**Q: Which Linux distribution are you using?**

I switched from OS X to Ubuntu GNOME.

**Q: What sort of challenges did you face, and how did you work around these challenges?**

For me, there was one challenge. When I first started "evaluating" (evaluating is probably too formal a word) different distributions I was trying to make them work exactly like my Mac, which always failed. I realized that I changed just about everything about the way I was working when I switched from Windows to OS X. I had to be willing to change the way I was doing some things for me to complete the move. I don't think this is a requirement for everyone who wants to switch to Linux. The point is this: you will likely need to be willing to change a few things.

My first step in my migration was to simplify the way I was working on my Mac first, then try to move again. This made everything a lot easier. I did away with a lot of the dedicated apps I was using (writing apps, GTD apps, note taking, etc). I started using open data formats for everything I was doing (basically, plain text for everything). I started using web-based email and the web interfaces for Twitter, cross-platform apps, etc.

**Q: What benefits have you seen after the switch?**

Since I made the switch I can say for certain that my setup has gotten a lot simpler. It would take me less than a day to rebuild my machine to the point where it is now. Another benefit has been performance (I reduced how much my machine swaps by reducing the "vm.swappiness" value to 10 in `/etc/sysctl.conf`). I still own a MacBook, which is the only laptop I have, and I never thought I would say that it felt slow. I spend a **lot** less time tweaking my machine to behave the way I want.

**Q: Is there anything else you'd like readers to know?**

I still own and use an iPhone and an Apple TV, so I haven't completely removed Apple from my computing landscape. I mention this because a lot of people think that it has to be "all or none," and I haven't found that to be the case.

I also wanted to mention a couple of very useful applications I've found along the way:

* [Enpass][link-3] is a great password manager (a reasonable replacement for 1Password on OS X, if you use that)
* [odrive][link-2] is a good cloud storage client that supports Linux
* [Netdata][link-1] is awesome for local performance monitoring

[link-1]: https://github.com/firehol/netdata
[link-2]: https://www.odrive.com
[link-3]: https://enpass.io
[xref-1]: {{< relref "2017-03-01-linux-migration-other-users-stories-pt1.md" >}}
[xref-2]: {{< relref "2017-03-06-linux-migration-other-users-stories-pt2.md" >}}
