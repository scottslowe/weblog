---
author: slowe
categories: Information
comments: true
date: 2017-10-25T12:00:00Z
tags:
- Linux
- Fedora
- macOS
- Productivity
title: 'The Linux Migration: Wrap-Up'
url: /2017/10/25/linux-migration-wrap-up/
---

As many readers are probably already aware, I embarked on a journey earlier this year to make Linux my primary laptop OS (see [this update][xref-1] from April of this year). That journey ended (for now) when I ordered a new 13" MacBook Pro just before VMworld US. In this post, I'd like to reflect a bit on my journey, and what it means for others who may be interested in similar journeys of their own.<!--more-->

So why the switch back to macOS? Well, it certainly does _not_ have anything to do with changes on the macOS side; all my concerns (first expressed [here][xref-2] in late November of 2012, almost five years ago) are still present. By all indications, the trend to "iOS-ify" macOS continues; this may be great for the masses but isn't so great for "power users" such as myself, in my humble opinion.

In the end, the decision to switch back to macOS really comes down to productivity. I think that [my July 2017 update post][xref-3] probably sums it up best: for me, trying to use Linux as my primary laptop OS was like "death from a thousand cuts." While I strongly prefer to use Linux as a server OS, trying to use Linux as a desktop OS wasn't quite so straightforward. I ended up running into a number of issues that were keeping me from being as productive as I wanted to be. Among others, these included:

* Difficulty with a stable and performant e-mail client
* Very limited CalDAV support in calendaring solutions
* Significant instability in GNOME Calendar
* Poorer-than-expected interoperability between Office 365 and LibreOffice

For each of these, I tried what I felt were reasonable workarounds. With regard to e-mail, for example, I tried both Thunderbird and Evolution. Both of them worked _okay_; neither worked _well_, and I ran into stability and reliability issues with both. With regard to calendaring, I set aside my preference for a dedicated calendar application and tried to leverage the calendaring inside Thunderbird and Evolution (GNOME Calendar was just too unstable). However, having already decided to separate myself from Google as much as possible (see [here][xref-4]), I just couldn't bring myself to have to go back to relying on Google for calendaring again due to the lack of CalDAV support.

Individually, none of these issues are insurmountable. Taken together, though, they were really impacting my productivity. On top of the other sacrifices I'd made to use Linux (limited Office 365 compatibility, more limited plain text task management system, no easy way to preview Markdown documents, more limited options for creating diagrams, etc.), having to fight these additional items was just too much.

So, as much as I _loved_ working in Linux, I felt that I was actively fighting to make it work. A phrase from [this article by Brian Ketelsen][link-1] is appropriate here, I think: "I prefer to develop completely in Linux. I prefer not to have to fight to use Linux as a primary desktop operating system." While I don't develop in Linux (more appropriate perhaps to say that I prefer to deploy on Linux), I think the sentiment is still applicable.

So what does this mean for others who might be pondering the switch to Linux as their primary OS? Truly and honestly, it's going to depend on the applications you use. For personal use only---where you're just surfing the web, managing personal e-mail, some light photo editing, etc.---you may be fine. For corporate use, the question is far less clear (in my opinion):

* If your employer relies heavily on Office 365, I think you'll find it challenging (unless you resort to using a VM extensively). 
* If your employer relies heavily on Microsoft-centric collaboration tools, I think you'll find it challenging to use Linux as your primary OS (again, unless you resort to using a Windows VM most of the time, in which case I would challenge whether you are really using Linux as your primary OS).
* If your employer relies heavily on Google Apps and you don't travel a lot, you'll probably be OK (assuming #1 is not also true).
* If your employer is more into "Linux-friendly" tools like Slack, IRC, standards-based e-mail/calendaring/contacts (IMAP/SMTP/CalDAV/CardDAV), and online platforms like wikis/blogs for information sharing, then you'll probably be fine (although calendaring may be an issue).

Despite the move back to macOS as my primary laptop OS, I'm still a huge fan of Linux, and I still plan to carefully track the development of Linux on the desktop to see how it continues to grow and evolve. If the various open source communities around desktop Linux---Fedora, GNOME, Mozilla, Ubuntu/Canonical---can address some of the shortcomings that I experienced, then I'd be happy to give it another go. Until then, I'll continue to use Linux on the server side and wait patiently for the right time to switch to it on my laptop as well.

[link-1]: https://brianketelsen.com/my-cross-platform-dev-setup-on-surface-laptop/
[xref-1]: {{< relref "2017-04-12-linux-migration-april-2017-progress-report.md" >}}
[xref-2]: {{< relref "2012-11-30-why-i-might-leave-os-x.md" >}}
[xref-3]: {{< relref "2017-07-10-linux-migration-july-2017-progress-report.md" >}}
[xref-4]: {{< relref "2013-12-04-divorcing-google.md" >}}
