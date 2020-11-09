---
author: slowe
categories: Information
comments: true
date: 2018-04-30T20:00:00Z
tags:
- Linux
- macOS
- CLI
- Markdown
- Encryption
- Writing
- Messaging
title: An Updated Look at My Multi-Platform Toolbelt
url: /2018/04/30/updated-look-at-multi-platform-toolbelt/
---

In early 2017 I posted about [my (evolving) multi-platform toolbelt][xref-1], describing some of the applications, standards, and services that I use across my Linux and macOS systems. In this post, I'd like to provide an updated review of that toolbelt.<!--more-->

* _Visual Studio Code:_ I switched from [Sublime Text][link-3] to [Visual Studio Code][link-1] during my latest migration to Fedora 27 on a Lenovo ThinkPad X1 Carbon. Since I'm also planning on expanding my coding skills with Golang, I felt that Visual Studio Code would be a better choice than Sublime Text. I'm still generating the majority of my content in Markdown (MultiMarkdown is the flavor that I generally use), and I've found Visual Studio Code to be pretty decent as a Markdown editor.

* _IMAP/SMTP:_ I've standardized on using IMAP/SMTP for all my e-mail accounts, which gives me quite a bit of flexibility in clients and OSes. It's very likely I've pretty much standardized on [Thunderbird][link-2] (which supports macOS, Linux, and Windows).

* _Unison:_ This [cross-platform file synchronization tool][link-4] helps keep my files in sync across my macOS and Linux systems.

* _Dropbox:_ [Dropbox][link-5] gives me access to non-confidential files from any of my devices or platforms (macOS, iOS, and Linux).

* _jrnl:_ [`jrnl`][link-7] is a CLI tool is used for journaling, and has built-in support for encryption (a plus for me). The nice thing about `jrnl` which is itself multi-platform (runs on macOS and Linux) is that it also works with [Day One][link-6], a popular journaling tool for macOS. I've since abandoned Day One (for a variety of reasons) and use only `jrnl` now on both Linux and macOS.

* _TaskPaper-formatted text files for task management:_ I've adopted the TaskPaper format for using plain text files for task management. Obviously I'm not using Sublime Text but Visual Studio Code instead, along with the Todo+ extension.

* _Firefox and Firefox Sync:_ For web browsing, I'm using [Mozilla Firefox][link-8] and Firefox Sync across Linux, macOS, iOS, and Android. I'll occasionally use [Google Chrome][link-18] as needed.

* _1Password X:_ In the past, I had to look at other password managers (like [Enpass][link-19]) because Agile Bits didn't have any Linux support for [1Password][link-9]. With the introduction of 1Password X (including a Firefox extension!), I'm sticking with 1Password as my password manager.

* _Google Calendar:_ As much as I'd prefer not to have to use Google, I've found that I don't really have much choice in the matter. Linux and GNOME support for CalDAV and CardDAV just isn't there, unfortunately---GNOME Calendar remains incredibly unstable. So, I'm in the process of moving all my calendaring over to Google Calendar and away from CalDAV with [fruux.com][link-10]. I'll do something similar for my contacts.

* _Slack:_ [Slack][link-11] has become a much larger part of my collaborative workflow, and so I'm using the "native" Slack client on both macOS and Linux. (I put native in quotes because it's using Electron and therefore isn't truly native to the platform.)

There are still a few areas where there are inconsistencies across platforms (for various reasons). For example, when it comes to local virtualization, I use [VirtualBox][link-13] on macOS and Libvirt (KVM) on Linux---but both of them are fronted by Vagrant, so the differences between the user experience are pretty minimal. (It's worth noting that I have completely moved away from [VMware Fusion][link-12] on all macOS systems, as the Vagrant user experience is still subpar, in my opinion. See [this post][xref-2] for more details.)

Similarly, I haven't standardized on a single IRC client across platforms (I'm using [Textual][link-16] on macOS and [HexChat][link-17] on Linux); I'm not yet sure there's any real benefit to standardizing on a single IRC client. (Feel free to educate me otherwise if you have experience in this regard.)

Finally, the same is true for graphical Git clients (which I rarely use, to be honest): on Linux I'm using [GitKraken][link-20], and on macOS I'm using [Tower][link-21].

I'm no longer using [Wire][link-14]; I just wasn't seeing any real benefit, and the use of tools like OTR with other platforms helps provide a level of secure messaging that is acceptable.

Well, that's it for now. I'll keep this post updated as my toolbelt evolves. In the meantime, if you have any suggestions for tools that I should evaluate, feel free to [hit me up on Twitter][link-15].

[link-1]: https://code.visualstudio.com/
[link-2]: https://www.mozilla.org/en-US/thunderbird/
[link-3]: http://www.sublimetext.com/
[link-4]: http://www.cis.upenn.edu/~bcpierce/unison/
[link-5]: https://www.dropbox.com/
[link-6]: http://dayoneapp.com/
[link-7]: http://jrnl.sh/
[link-8]: https://www.mozilla.org/en-US/firefox/
[link-9]: https://1password.com/
[link-10]: https://fruux.com/
[link-11]: https://slack.com/
[link-12]: http://www.vmware.com/products/fusion.html
[link-13]: https://www.virtualbox.org/
[link-14]: https://wire.com/
[link-15]: https://twitter.com/scott_lowe/
[link-16]: https://www.codeux.com/textual/
[link-17]: https://hexchat.github.io/
[link-18]: https://www.google.com/chrome/index.html
[link-19]: https://www.enpass.io/
[link-20]: https://www.gitkraken.com/
[link-21]: https://www.git-tower.com/
[xref-1]: {{< relref "2017-01-03-my-evolving-multi-platform-toolbelt.md" >}}
[xref-2]: {{< relref "2016-09-28-why-now-using-virtualbox-with-vagrant.md" >}}
