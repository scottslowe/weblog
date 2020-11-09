---
author: slowe
categories: Information
comments: true
date: 2017-04-12T00:00:00Z
tags:
- Linux
- Fedora
- macOS
title: 'The Linux Migration: April 2017 Progress Report'
url: /2017/04/12/linux-migration-april-2017-progress-report/
---

In December 2016, I kicked off a migration to Linux (from macOS) as my primary laptop OS. In the nearly 4 months since the [initial progress report][xref-1], I've published a series of articles providing updates on things like [which Linux distribution][xref-2] I selected, how I'm handling [running VMs][xref-3] on my Linux laptop, and integration with corporate collaboration systems ([here][xref-4], [here][xref-5], and [here][xref-6]). I thought that these "along the way" posts would be sufficient to keep readers informed, but I've had a couple of requests in the last week about how the migration is going. This post will help answer that question by summarizing what's happened so far.<!--more-->

Let me start by saying that I am _actively using a Linux-powered laptop as my primary laptop right now,_ and I have been doing so since early February. All the posts I've published so far have been updates of how things are going "in production," so to speak. The following sections describe my current, active environment.

## Linux Distribution

In my initial progress report, I'd tentatively chosen to use [Ubuntu][link-1] 16.04 LTS ("Xenial Xerus"). However, a short while later I switched to [Fedora][link-2] 25, and have settled on Fedora as my primary laptop OS. (See [this post][xref-2] for more details.)

The only issue with the Fedora installation so far has been that I didn't set up full disk encryption when I installed; it looks like I'll need to reinstall Fedora in order to enable full disk encryption.

## Hardware

The laptop on which I'm running Fedora 25 is a Dell Latitude E7370 (see [my review][xref-7]) with 16GB RAM, 512GB NVMe SSD, and a 3200x1800 touch screen. Every piece of hardware worked _perfectly_ out of the box---wireless, touchpad, touch screen, USB-C/Thunderbolt docking station, etc. I currently use an external 27" monitor, external keyboard, and external mouse all connected via a USB-C/Thunderbolt docking station when I'm in the office, and docking/undocking the laptop works almost flawlessly. I say "almost" only because I've had a couple instances (out of the dozens of times I've docked or undocked) that it didn't work as expected. To be fair, though, I've seen the same behavior with macOS.

## Applications

For the most part, things are where they were at the time of my initial progress report:

* _Markdown:_ A great deal of my content was already being generated in Markdown, and I was already using [Sublime Text][link-5] 3 on macOS. On Linux, I still generate most everything in Markdown, and I still use Sublime Text 3 as my preferred text editor.
* _Internet applications:_ [Firefox][link-6] handles my web browsing, but I also have [Google Chrome][link-7] installed for cross-browser testing. I use [HexChat][link-8] for IRC, the [Linux Slack client][link-9], and [Pidgin][link-10] for instant messaging.
* _Cloud storage/sync:_ [Dropbox][link-11] works fine, although the UI integration could be much better. I'm also using a tool called [ODrive][link-12] to get connectivity to OneDrive for Business.
* _Basic office productivity:_ I'm using [LibreOffice][link-4] (included with most Linux distributions these days). I have run into a few compatibility issues with Office 2016/Office 365, particularly regarding presentations. For my own presentations, I've moved to a Markdown-based solution (more details [here][xref-8]).
* _Graphics:_ The combination of [Inkscape][link-13] and [GIMP][link-14] take care of my graphics needs. I was able to create SVG (Scalable Vector Graphics) versions of some of my OmniGraffle files (but not all), which I can edit/modify in Inkscape as needed.
* _Mind mapping:_ [XMind][link-16] fills the need here. There's nothing I can do about my old MindNode maps, but all new maps are created in XMind. So far, the free edition has met my needs, but it may be necessary to upgrade at some point (mostly for improved import/export functionality).
* _E-mail:_ I'm using [Thunderbird][link-3] with a small collection of add-ons to handle both corporate and personal e-mail (refer to [this post][xref-4] for more details). I did _not_ import the 10+ years of archived mail stored in Apple Mail, keeping that on my Mac Pro instead (more on that later).
* _Task management:_ I switched to a task management system that is completely based on plain text files using a variant of the TaskPaper format (it's not 100% TaskPaper compatible). This allows me to handle task management via Sublime Text, Dropbox, and [Meld][link-15] (a file differencing tool). See [this post][xref-9] for more details.
* _Time management:_ Time management (calendaring/scheduling) is difficult, due to issues described [here][xref-5]. I'm currently using the Lightning add-on in Thunderbird, as well as GNOME Calendar. Thunderbird handles my corporate calendar, and GNOME Calendar shows corporate and personal calendars. On the personal calendar side, I've had to switch back to Google from a third-party CalDAV provider; thus, GNOME Calendar syncs with Google Calendar and with my corporate calendar.
* _Password management:_ I switched from 1Password to [Enpass][link-17]. I've heard that 1Password may be offering a Linux version this year; I'll evaluate switching back if that happens. I'm using Enpass on my macOS, Linux, and iOS devices.
* _Corporate connectivity:_ I have the Linux client for VMware Horizon installed (to access my hosted corporate desktop), and I use `vpnc` to connect to our corporate VPN. I also maintain a Windows 10 VM with corporate VPN connectivity for other issues I can't resolve using Linux.
* _Social media:_ I use [Corebird][link-18] for Twitter access from Linux when I'm traveling.

## Where I Still Use macOS

macOS hasn't completely disappeared from my computing landscape; I have a well-equipped Mac Pro in my office that I still use for some things. (When I'm traveling, though, it's pretty much 100% Linux---the exception is the occasional need to boot the corporate Windows 10 VM.) Here are the areas where I'm still using macOS:

* Podcast recording (I haven't found a Linux replacement for Audio Hijack, and Skype on Linux is a bit iffy)
* Viewing archived e-mail (still sits in Apple Mail on my Mac Pro)
* Viewing OmniGraffle documents that I couldn't convert to SVG
* Working with any old MindNode mind maps (I wrote a script to extract the JPEG preview from the MindNode file bundle, so I can view the preview on Linux)
* Social media when I'm in the office (Tweetbot is far more powerful than Corebird)
* Any virtualization testing specific to VMware Fusion (like testing a VMware-formatted Vagrant box, for example)

I also use my macOS system for general web browsing when I'm in the office, since there's no sense in letting its 27" Thunderbolt Display go to waste. By and large, though, most everything is done from my Linux laptop.

## Summary

As I mentioned in [my last post][xref-6] on corporate collaboration, whether or not Linux on the desktop will work _for you_ will depend a lot on your employer and the systems your employer uses. I've been able to make this work, but not everyone will be able to do the same.

I'll keep posting "in progress" updates as well as summary updates like this one when there is useful information to share. In the meantime, if you have questions, feedback, or suggestions, feel free to connect with me [on Twitter][link-19].



[link-1]: https://www.ubuntu.com/desktop
[link-2]: https://getfedora.org/
[link-3]: https://www.mozilla.org/en-US/thunderbird/
[link-4]: https://www.libreoffice.org/
[link-5]: http://www.sublimetext.com/
[link-6]: https://www.mozilla.org/en-US/firefox/
[link-7]: https://www.google.com/chrome/
[link-8]: https://hexchat.github.io/
[link-9]: https://slack.com/downloads/linux
[link-10]: https://pidgin.im/
[link-11]: https://www.dropbox.com/
[link-12]: https://www.odrive.com/
[link-13]: https://inkscape.org/en/
[link-14]: https://www.gimp.org/
[link-15]: http://meldmerge.org/
[link-16]: http://www.xmind.net/
[link-17]: https://www.enpass.io/
[link-18]: http://corebird.baedert.org/
[link-19]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2016-12-16-linux-migration-initial-progress-report.md" >}}
[xref-2]: {{< relref "2017-02-01-linux-migration-final-distro-selection.md" >}}
[xref-3]: {{< relref "2017-02-08-linux-migration-virtualization-provider.md" >}}
[xref-4]: {{< relref "2017-03-21-linux-migration-corp-collab-pt1.md" >}}
[xref-5]: {{< relref "2017-03-27-linux-migration-corp-collab-pt2.md" >}}
[xref-6]: {{< relref "2017-04-03-linux-migration-corp-collab-pt3.md" >}}
[xref-7]: {{< relref "2017-01-30-review-dell-latitude-e7370.md" >}}
[xref-8]: {{< relref "2017-03-07-linux-migration-creating-presentations.md" >}}
[xref-9]: {{< relref "2017-01-19-plain-text-productivity-redux.md" >}}
