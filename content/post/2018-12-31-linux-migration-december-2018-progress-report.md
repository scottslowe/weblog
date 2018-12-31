---
author: slowe
categories: Information
comments: true
date: 2018-12-31T09:30:00Z
tags:
- Linux
- Fedora
- Productivity
- Messaging
- Hardware
title: 'The Linux Migration: December 2018 Progress Report'
url: /2018/12/31/linux-migration-december-2018-progress-report/
---

In December 2016, I kicked off a migration from macOS to Linux as my primary laptop OS. Throughout 2017, I chronicled my progress and challenges along the way; links to all those posts are found [here][xref-1]. Although I stopped the migration in August 2017, I restarted it in April 2018 when I left VMware to join Heptio. In this post, I'd like to recap where things stand as of December 2018, after 8 months of full-time use of Linux as my primary laptop OS.<!--more-->

I'll structure this post roughly as a blend of the formats I used in my [April 2017][xref-5] and [July 2017][xref-4] progress reports.

## Hardware

Readers may recall that I was using a Dell Latitude E7370 (see [my E7370 hardware review][xref-2]) up until August 2017, when I put the Linux migration on hold indefinitely due to productivity concerns. Upon moving to Heptio, I switched to a Lenovo ThinkPad X1 Carbon (see [here][xref-3] for my review of the X1 Carbon---the "TL;DR" is that I _love_ it). In my home office, the X1 Carbon connects to a USB-C expansion hub that provides connectivity to a 34" 21:9 ultrawide curved monitor, external HD webcam, and a USB headset for Zoom meetings. I also recently converted my Mac Pro to Linux as well (see [this post][xref-6] for details); that's a workstation with dual quad-core Xeon CPUs, 24GB of RAM, a 512GB SSD, a 1TB hard drive, and a 700GB PCIe SSD from Micron, also connected to a 34" 21:9 ultrawide curved monitor.

## Linux Distribution

Early (very early) in the migration I thought I would end up using Ubuntu 16.04, but I switched to [Fedora][link-1] and haven't looked back. I started with Fedora 25 on the E7370, switching to Fedora 27 on the X1 Carbon and later upgrading to Fedora 28. The Mac Pro started out with Fedora 27 ([this post][xref-6] outlines the reasons why) and was upgraded to Fedora 28.

## Applications

By and large, application usage remains mostly unchanged from earlier:

* _Markdown:_ I switched from [Sublime Text][link-2] to [Visual Studio Code][link-3], but otherwise my Markdown workflows remain largely the same. I still use Markdown for the vast majority of my content creation.
* _Browsing:_ While I have [Google Chrome][link-7] installed, I use [Firefox][link-4] for the vast majority of my browsing (I don't care for the changes Chrome made with regard to signing you into the browser when you sign into Google). I make regular use of the Firefox Multi-Account Containers add-on to streamline some browser-based workflows while also protecting my privacy.
* _Chat/Instant messaging:_ I continue to use [the Slack Linux client][link-5] and [Pidgin][link-6]. For IRC---though I rarely use it these days---I have [HexChat][link-8], as before.
* _Cloud storage/sync:_ GNOME offers built-in integration for Google Drive, and I'm still using [Dropbox][link-12] for non-confidential information. I'll probably have to start using [ODrive][link-13] again for access to OneDrive.
* _Basic office productivity:_ I'm still using [LibreOffice][link-15], though it's a newer version.
* _Graphics:_ No changes here, except some occasional use of LibreOffice Draw.
* _Mind mapping:_ I'm still using [XMind][link-14], and still evaluating whether a Pro license is needed or not. I haven't had a strong need for the extra features yet.
* _E-mail:_ I continue to use [Thunderbird][link-9], and I resolved the nagging performance concerns that I noted during the July 2017 progress report (these appear to have been caused by some TCP timeouts related to the NAT configuration on my ASA 5505). I've settled on IMAP/SMTP for all e-mail access. Having recently re-joined VMware via the Heptio acquisition, I'm currently using [TbSync][link-10] with the Active Sync add-on for calendar and contacts access (it also provides access to the Global Address List). The TbSync CalDAV/CardDAV provider enables access to the existing CalDAV- and CardDAV-based sync service I'm using on my mobile devices, providing a smoother user experience.
* _Task management:_ Aside from switching from Sublime Text to Visual Studio Code, this solution remains the same (plain text-based files on Dropbox, changes reconciled via a graphical diff program).
* _Calendaring/time management:_ I gave up on GNOME Calendar and settled for leveraging Lightning inside Thunderbird. Via the TbSync add-on, I have access to corporate and personal calendars. Access to Google Calendar (which is what we were using at Heptio) was and is problematic; I managed to get read-only access from Thunderbird, but read/write access was only through the web interface.
* _Password management:_ I switched back to [1Password][link-11], leveraging the 1Password X add-on for Firefox for access from my Linux systems.
* _Corporate connectivity:_ This wasn't needed at Heptio, but now that I'm back at VMware I'm back to using the Linux client for VMware Horizon and `vpnc` (with integration into GNOME) for VPN connectivity.
* _Social media:_ I'm just using the Twitter web site; with the API changes, there aren't really any other options on Linux.

So what's working well with this configuration?

* Calendar access via TbSync seems to work really well; I'm really pleased that the CalDAV/CardDAV provider for TbSync works as seamlessly as it does.
* As I mentioned above, the performance issues I was seeing with e-mail seem to have been resolved. I'm not seeing the same lockups and delays that I was seeing in 2017.
* Switching to the [Materia theme][link-16] for GNOME/GTK has relieved the eyestrain issues I reported in [my July 2017 update][xref-4].

What's not working well?

* Now that I'm back at VMware via the Heptio acquisition, I'm moving back into a Microsoft-heavy environment. I anticipate I'll run into compatibility issues between LibreOffice and Office 365, as I did before, though there is a chance that the newer version of LibreOffice will work better. It's still too early to tell yet.

## Where I Still Use macOS

With [the migration of my Mac Pro to Fedora 28][xref-6], that leaves my 2017 MacBook Pro as the only remaining macOS-based system. I will still use it for podcast recording, viewing archived e-mail, and viewing documents in formats that I couldn't/didn't translate to a Linux equivalent (OmniGraffle, OmniOutliner, and MindNode, primarily). I'll also use it to transcode media into industry-standard formats (MP3 and MP4/H.264) that my Linux systems can use. Aside from that, it sees very little regular use.

## Summary

Through a combination of technology-related improvements in various applications plus personal growth on my part, I've been able to make Linux my full-time primary laptop OS since April 2018. Hopefully, moving back to VMware and its Microsoft-centric environment won't present the same kind of insurmountable hurdles I saw last time. Time will tell whether my hopes are valid, but I do want to do a better job of keeping readers informed about how things are going overall. As always, if you have questions, comments, or suggestions, I encourage you to [contact me on Twitter][link-99].

[link-1]: https://getfedora.org
[link-2]: http://www.sublimetext.com/
[link-3]: https://code.visualstudio.com/
[link-4]: https://www.mozilla.org/en-US/firefox/
[link-5]: https://slack.com/downloads/linux
[link-6]: https://pidgin.im/
[link-7]: https://www.google.com/chrome/
[link-8]: https://hexchat.github.io/
[link-9]: https://www.mozilla.org/en-US/thunderbird/
[link-10]: https://github.com/jobisoft/TbSync
[link-11]: https://1password.com/
[link-12]: https://www.dropbox.com/
[link-13]: https://www.odrive.com/
[link-14]: http://www.xmind.net/
[link-15]: https://www.libreoffice.org/
[link-16]: https://github.com/nana-4/materia-theme
[link-99]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2018-12-23-the-linux-migration-series.md" >}}
[xref-2]: {{< relref "2017-01-30-review-dell-latitude-e7370.md" >}}
[xref-3]: {{< relref "2018-04-13-review-lenovo-thinkpad-x1-carbon.md" >}}
[xref-4]: {{< relref "2017-07-10-linux-migration-july-2017-progress-report.md" >}}
[xref-5]: {{< relref "2017-04-12-linux-migration-april-2017-progress-report.md" >}}
[xref-6]: {{< relref "2018-12-19-running-fedora-on-my-mac-pro.md" >}}
