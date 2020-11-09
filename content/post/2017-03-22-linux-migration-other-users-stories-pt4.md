---
author: slowe
categories: Information
comments: true
date: 2017-03-22T00:00:00Z
tags:
- Linux
- Productivity
- Apple
- Debian
- Ubuntu
- Collaboration
title: 'The Linux Migration: Other Users'' Stories, Part 4'
url: /2017/03/22/linux-migration-other-users-stories-pt4/
---

This post is part of a series of posts sharing other users' stories about their migration to Linux as their primary desktop OS. As I mentioned in [part 1 of the series][xref-1], there seemed to be quite a bit of pent-up interest in using Linux as your primary desktop OS. I thought it might be helpful to readers to hear not just about my migration, but also about others' migrations. You may also find it interesting/helpful to read [part 2][xref-2] and [part 3][xref-3] of this series for more migration stories.

This time around I'll share with you some information from Ajay Chenampara about his Linux migration. Note that although these stories are all structured in a "question-and-answer" format, the information is unique---just as each person's migration and the reasons for the migration are unique.

**Q: Why did you switch to Linux?**

I have been a long-time Linux user, but I have only really used it as a media server or for casual browsing. Recently, I inherited a 7 year old laptop from my wife, and decided to focus on making it my primary system for writing my blog and for OSS efforts. Plus, I kept hearing about [Debian "Jessie"][link-6] from the blogosphere, so I wanted to give that distribution a shot as well.

Additionally, I recently changed jobs. At my new job, I have the option to use any operating system (OS), and I naturally gravitated toward Linux.

**Q: What hardware are you using with Linux?**

At first, I was using an older laptop that I inherited from my wife. I used this to get started on my Linux migration journey, focusing on writing my blog and other open source efforts. After changing jobs, though, I did some in-depth research and ended up a Lenovo P50 with a 6820HQ processor and 40GB of RAM ([here's a thread][link-4] of other people's experience with Linux on the P50). I tend to run lots of VMs, so the extra RAM is really helpful.

**Q: What were you using before switching to Linux?**

At my previous job, I had been using an Apple MacBook Pro. I realized that 80-90% the applications I used on the MacBook Pro were pretty much identical to what I'd use on Linux. This made the switch a no-brainer, especially given the additional control I get on the system.

The applications I use mostly are:

1. [Emacs][link-8]
    - It serves as my primary text editor
    - It's also my organizer (org-mode for the win!)
    - My primary integrated development environment (IDE) (projectile + python + magit is pretty much what I use)
    - My primary slide/presentation editor (org-reveal + reveal.js is awesome)
2. Vim (I am a long time vim user and sometimes just revert to vim)
3. O365 + OmniGraffle (note: I haven't really tested out the O365 equivalent on the Linux laptop, given that it is not my work. For drawings, I think LibreOffice is satisfactory, at least for now)

**Q: What sort of challenges did you encounter, and how did you overcome them?**

Wi-Fi and WebEx were the worst. On the older laptop, I had a Broadcom wireless chip and used the b43 kernel module. However, in spite of having the firmware, I kept running into random Wi-Fi drops. At this point, I have tried so many things that I no longer know which step actually fixed it, but for now, I have reliable connectivity.

Getting WebEx to work was the other big-time killer. I first went with Chromium (open source version of Chrome), and then realized that the 32-bit [Iceweasel][link-1] plugin was the way to go. (For those who don't know, Iceweaseal is the Debian-branded version of Firefox.) However, if you are going down this path, please remember to uninstall the old plugin. Without that step, the new plugin is of no help.

Going with a basic distro, however, I had to install a bunch of software to get my environment the way I liked it (Docky, scudcloud, terminator, tmux, synapse, etc.). None of this was too complex. I do love the Greybeard theme on XFCE. Given that I was using a 7-year-old laptop at the time, it really flew!

On my new Lenovo, I installed a pre-release testing version of [Debian "Stretch"][link-7] (the next major version of Debian) after creating a recovery disk for the installation of Windows 10 Pro that came on the laptop. Unfortunately, the HDMI port wouldn't work. I tried all the various suggestions I found online (such as [this thread][link-5]), but realized I would need to pivot. I installed [Xubuntu][link-3]---I wanted to preserve my XFCE experience---and found that the external display now worked. After spending a fair amount of time customizing my environment, installing applications, setting up my "dot-files" (configuration files), and even getting WebEx Meeting Center working, I then realized that there was no support for WebEx personal meeting rooms on Linux.

This was a problem for me. I spent another afternoon and evening restoring Windows 10 Pro---but only long enough to perform a physical-to-virtual (P2V) conversion into a disk image. I then re-loaded Xubuntu and spun up my Windows 10 Pro instance using VMware Player.

To this moment, WebEx has been the biggest thorn in my Linux migration. Even with Meeting Center, which works natively, I cannot use the computer audio. [This document][link-3], originally written in 2015 and updated in 2017, still proclaims no support for Linux. I'm baffled why that's the case.

This forces me into a bit of an ugly hack when I need to share my Linux desktop. I have to start the x11vnc server (so that I can console into the Linux instance via RDP from Windows), spin up the Windows VM, and RDP back to the host. It works, but it's definitely a hack.

**Q: What benefits have you seen after the switch?**

Well, for one I have a beautiful work environment that requires minimal mouse movement and is really fast! I have a great deal of power and control over the environment, and I have an environment that makes it easier to contribute back to the community and learn more about how computers and operating systems work.

**Q: Anything else you'd like others to know?**

You should really try out emacs org-mode! [Here's a video][link-9] that will help you get started.

[link-1]: https://wiki.debian.org/Iceweasel
[link-2]: https://xubuntu.org/
[link-3]: https://help.webex.com/docs/DOC-3921
[link-4]: https://forums.lenovo.com/t5/Linux-Discussion/P50-P70-linux-experiences/td-p/2251327
[link-5]: https://forums.lenovo.com/t5/ThinkPad-P-and-W-Series-Mobile/Lenovo-P50-HDMI-not-working/td-p/2264525
[link-6]: https://www.debian.org/releases/jessie/
[link-7]: https://www.debian.org/releases/stretch/
[link-8]: https://www.gnu.org/software/emacs/index.html
[link-9]: https://www.youtube.com/watch?v=SzA2YODtgK4
[xref-1]: {{< relref "2017-03-01-linux-migration-other-users-stories-pt1.md" >}}
[xref-2]: {{< relref "2017-03-06-linux-migration-other-users-stories-pt2.md" >}}
[xref-3]: {{< relref "2017-03-10-linux-migration-other-users-stories-pt3.md" >}}
