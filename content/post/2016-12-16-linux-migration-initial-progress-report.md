---
author: slowe
categories: Information
comments: true
date: 2016-12-16T00:00:00Z
tags:
- Linux
- CLI
- Ubuntu
- macOS
- Fedora
title: 'The Linux Migration: Initial Progress Report'
url: /2016/12/16/linux-migration-initial-progress-report/
---

About 4 years ago, I discussed some changes in the Apple ecosystem that might lead me to [move away from OS X][xref-1]. To be honest, I've made only token efforts since that time to actually migrate away, even though the forces that I described in that post are still in full effect. In fact, some might say that the "iOS-ification" of OS X (now rebranded as "macOS") is even stronger now. As a result, I've stepped up my work on a Linux migration, and I'm happy to report that I've made some progress.

Here's a quick update on where things stand so far.

## Linux Distribution

I've looked at a fair number of Linux distributions. I tried [Elementary OS][link-2], which [some have raved about][link-1] but which I found too simplistic. I also went back and looked again at Ubuntu derivatives like [Linux Mint][link-3]. Given that Ubuntu is itself derived from Debian, I also took a look at [Debian "Jessie"][link-11]. Finally, I tested [Fedora 25][link-4]. For a number of reasons---which I'll describe in more detail in a moment---I've settled on [Ubuntu 16.04][link-5].

So, why Ubuntu 16.04 "Xenial Xerus"? Keep in mind that the reasons I list below are _my_ reasons, and may not apply to you or your situation. I also realize that some of these points tread on "religious war" territory, so I'll just say again that it's up to each individual to find what works best for her/him.

* _Hardware support:_ Given that I'm a current macOS user, it's quite likely that I'll press Apple hardware into service first. With that in mind, Ubuntu 16.04 (which I'll just refer to as Xenial from here on) does a good job of supporting hardware (I'm writing this from Ubuntu 16.04 running natively on a slightly older MacBook Pro, and everything is working flawlessly). Wireless works out of the box, and the hardware-specific keys for adjusting sound, keyboard backlighting, and screen brightness work as expected.

* _Performance:_ Based on highly scientific (read: utterly subjective) tests, I'm pretty pleased with Xenial's performance, both virtual (using both VMware Fusion and VirtualBox) and on bare (laptop) metal. Graphics performance is good, and the system "feels" really responsive overall.

* _Leading (but not bleeding) edge:_ Xenial runs a fairly recent Linux kernel (4.4), which is new enough to provide good support for hardware and other features, but not so new as to be bleeding edge.

* _User interface:_ Despite some misgivings over the Unity interface, I've found that I prefer it to GNOME 3 and Classic GNOME. I know that many people will disagree with me here, and that's fine. It's what works for _me_.

(Update: After a few weeks and some additional testing, I've switched to Fedora 25 with GNOME 3. See [this post][xref-3] for more details.)

## Data Formats

This is a big issue, and one that anyone seeking to migrate off macOS (or Windows, for that matter) must address. I'd already made the move to [Markdown][link-6] (MultiMarkdown, technically) for a great deal of my content generation, and I'd made the switch to [Sublime Text][link-7] (ST) as my primary editor. ST is a cross-platform app, so I can switch to ST on Linux and preserve the majority of my user experience.

That being said, there were (and are) still a number of areas of concern. Fortunately, I've made some progress there:

* _OmniGraffle:_ I've found that I can export [OmniGraffle][link-12] drawings to SVG and use Inkscape on Linux to work with the SVG images.
* _1Password:_ This one really had me worried; [1Password][link-13] is an invaluable tool. However, I've found an application called [Enpass][link-8] that not only looks and feels a lot like 1Password, but also allowed me to import my 1Password vault (the import wasn't perfect, but it's better than recreating everything). With support for a variety of platforms and browsers, this seems like a really good option. (Just in case the folks from Agile Bits are reading: make a Linux version of 1Password and I'll definitely buy it, no questions asked.)
* _Microsoft Office:_ [LibreOffice][link-9] should handle the majority of what I need there (I don't typically work with really complicated documents), and I plan to keep a Windows VM with Office installed just in case (though I don't want to rely too heavily on it).

Some data format issues have yet to be resolved, though:

* _Apple iWork:_ There's not really much that can be done here aside from export them to PDF or the equivalent Office format. Moving forward, I'll use either Markdown or LibreOffice. I especially plan to focus more heavily on Markdown-based presentations.
* _MindNode:_ I can export these to PDF (or an image format) or OPML (with a loss of formatting), but I haven't yet identified a replacement app on Linux.
* _OmniOutliner:_ I moved away from the proprietary file format used by [OmniOutliner][link-14] several years ago, instead embracing OPML. However, I have yet to find a reasonable outlining solution for Linux that will also accept OPML.
* _OmniFocus:_ I [tried plain text task management][xref-2] in the past, and ended up coming back to [OmniFocus][link-15]. I'm going to try it again (OmniFocus can export to a very TaskPaper-like format), with using an online service like Remember The Milk (among others) as a backup plan. (As an aside, let me just say that the Omni Group folks do _amazing_ work---good for them, but bad for folks like me who want to move off their apps.)
* _Corporate collaboration:_ Need I say anything further? I'd prefer not to rely heavily on having a Windows VM around, but I've yet to find a good solution here. Mail isn't an issue---I can easily do IMAP/SMTP with Office 365 using [Thunderbird][link-10]---but calendaring and conferencing are different stories, especially given that my employer is heavily embracing a Microsoft-centric collaboration suite.

As you can see, there's still lots of work to do and lots of questions to answer, but progress is happening. I'll continue to post updates here on the blog as this effort develops.

[link-1]: https://www.linux.com/learn/elementary-os-loki-has-arrived
[link-2]: http://elementary.io/
[link-3]: https://www.linuxmint.com/
[link-4]: https://getfedora.org/
[link-5]: https://www.ubuntu.com/desktop
[link-6]: https://en.wikipedia.org/wiki/Markdown
[link-7]: http://www.sublimetext.com/
[link-8]: https://enpass.io/
[link-9]: https://www.libreoffice.org/
[link-10]: https://www.mozilla.org/en-US/thunderbird/
[link-11]: https://www.debian.org/releases/stable/
[link-12]: https://www.omnigroup.com/omnigraffle/
[link-13]: https://1password.com/
[link-14]: https://www.omnigroup.com/omnioutliner/
[link-15]: https://www.omnigroup.com/omnifocus
[xref-1]: {{< relref "2012-11-30-why-i-might-leave-os-x.md" >}}
[xref-2]: {{< relref "2015-04-06-plain-text-productivity-experiment.md" >}}
[xref-3]: {{< relref "2017-02-01-linux-migration-final-distro-selection.md" >}}
