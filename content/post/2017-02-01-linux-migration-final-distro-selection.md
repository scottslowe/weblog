---
author: slowe
categories: Information
comments: true
date: 2017-02-01T00:00:00Z
tags:
- Linux
- CLI
- Fedora
title: 'The Linux Migration: Final Linux Distro Selection'
url: /2017/02/01/linux-migration-final-distro-selection/
---

In my [Linux migration initial progress report][xref-1], I provided an early assessment of the Linux distribution that I thought I would use moving forward. At that time, I had selected [Ubuntu][link-1]. Since that time, though, I've pivoted a bit and selected a different Linux distribution as the operating system (OS) for my primary laptop moving forward. In this post, I'd like to describe why I selected [Fedora][link-2].

My original reasons for selecting Ubuntu 16.04 were as follows:

* Hardware support
* Performance
* Leading but not bleeding edge
* User interface

These are all valid reasons, but as I continued to compare Ubuntu against Fedora 25 I realized that some of these factors weren't as critical as I'd originally thought:

* _Hardware support:_ I initially targeted Ubuntu because it runs really well on Apple hardware. Fedora, on the other hand, doesn't run quite as well on Apple hardware. Since I'm coming from the OS X world, I initially placed some emphasis on support for Apple hardware. The reality is, though, that I need a Linux distribution that does a great job of supporting my new work laptop, not one of my leftover Mac laptops. My experience with Fedora 25 on the Dell E7370 has been extremely positive---everything worked out of the box without any additional effort.

* _Performance:_ In my initial testing---conducted via virtual machines (VMs) under both [VirtualBox][link-3] and [VMware Fusion][link-4] as well as on an older Mac laptop---Ubuntu was both faster and more stable than Fedora. Continued testing led me to believe that the bare metal performance issues I was seeing could be attributed to poor hardware support (I was conducting these tests on an older Mac laptop); more VM testing under Fusion showed Fedora to be far more stable than I'd previously seen. My experience with Fedora 25 on the Dell E7370 further supports that conclusion: I've seen very few crashes and performance has been outstanding.

* _Leading but not bleeding edge:_ This is definitely a valid point, but in the case of working with a newer laptop (such as the E7370) having an older kernel and older packages actually works against me.

* _User interface:_ There are a number of things I really like about Unity (the HUD is one thing I find very useful). My initial experience with GNOME 3 wasn't too positive (a crashing and unstable Fedora on older Mac hardware), but upon finding Fedora to be more stable in other environments I came to like GNOME 3. Finding [the Numix theme(s)][link-5] made it even better, and a careful selection of [GNOME Extensions][link-6] further improved the environment. There are still a few things I don't like, but isn't that the case with pretty much every desktop environment?

There was one other consideration with which I was struggling. I wanted to work with a Linux distribution that was more "technically pure." (This isn't the best term, but it's the only one I can come up with at the moment.) I've used Ubuntu for quite a while, and I've always felt that Ubuntu makes some tradeoffs between "technical purity" and practicality/usability. These tradeoffs are what makes Ubuntu run better on Apple hardware, for example. Additionally, it's clear that [systemd][link-7] is a key part of Linux's future (whether you like it or not). Subjectively speaking, Ubuntu's systemd implementation felt "bolted on," somehow less complete or less comprehensive than Fedora's implementation. Maybe that's because systemd has been around on Fedora a lot longer than Ubuntu; I don't know.

In any case, for all these reasons I really felt that Fedora was better suited for me _at this point_ in my Linux journey. Ubuntu, with its focus on immediate usability, had served me well thus far, but now it was time for something new.

In upcoming posts on the Linux migration, I'll tackle updates on topics like corporate collaboration. Until then, feel free to hit me up [on Twitter][link-8] if you have any questions or feedback.



[link-1]: https://www.ubuntu.com/
[link-2]: https://getfedora.org/
[link-3]: https://www.virtualbox.org/
[link-4]: http://www.vmware.com/products/fusion.html
[link-5]: https://numixproject.org/
[link-6]: https://extensions.gnome.org/
[link-7]: https://www.freedesktop.org/wiki/Software/systemd/
[link-8]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2016-12-16-linux-migration-initial-progress-report.md" >}}
