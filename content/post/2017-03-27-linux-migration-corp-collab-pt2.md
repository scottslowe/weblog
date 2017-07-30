---
author: slowe
categories: Information
comments: true
date: 2017-03-27T00:00:00Z
tags:
- Productivity
- Linux
- Fedora
- Collaboration
- Exchange
title: 'The Linux Migration: Corporate Collaboration, Part 2'
url: /2017/03/27/linux-migration-corp-collab-pt2/
---

This post is part 2 in a series of posts describing how I've integrated my [Fedora Linux][link-5] laptop into my employer's corporate communication and collaboration systems. [Part 1][xref-1] tackled e-mail; this post tackles the topic of calendaring and scheduling. Unlike e-mail, which was solved relatively easily, this issue is one that I don't consider fully solved.

As I mentioned in part 1, my employer uses [Office 365][link-1] (O365). While O365 supports standard protocols like IMAP and STMP for mail, it does _not_ support standard protocols like CalDAV for calendaring. This means that Linux users like me are left with only a few options:

1. You can use [Mozilla Thunderbird][link-2] with the [Lightning add-on][link-4], but you'll also need an Exchange provider. (The paid Exquilla add-on only handles mail and contacts, not calendaring. There's a Lightning provider available [here][link-12], but I haven't tested it.)
2. You can use [Evolution][link-3].
3. You can use [GNOME Calendar][link-6] (which leverages the Evolution back-end along with Evolution's support for Exchange Web Services [EWS]).
4. You can use [Microsoft Outlook][link-7], either via a VM (or possibly via WINE, though I haven't tested the latter approach).

I'd already ruled out Evolution for e-mail, so it didn't make a lot of sense (to me, at least) to use Evolution for calendaring. Further, I ideally wanted a solution that would let me leverage CalDAV for my personal calendars as well as handling my corporate calendar (this was a luxury to which I'd grown accustomed on OS X). 

GNOME Calendar provides the standalone calendar experience I was seeking, and can handle my corporate calendar via the Evolution EWS support. However, there are a number of shortcomings:

* GNOME Calendar doesn't have CalDAV support, so I can't use it to manage my personal calendars.
* There's no timezone support. This can make it difficult when collaborating globally with co-workers in multiple time zones.
* GNOME Calendar has only annual and monthly views. A weekly view would be preferable (it's my understanding that the next release of GNOME adds a weekly view to GNOME Calendar).
* No way to handle meeting requests directly in the application; you have to go to your mail client for that. This meant that even though I wanted to manage my calendar with GNOME Calendar, I still needed the Lightning add-on in Thunderbird (otherwise Thunderbird doesn't understand/can't display meeting requests).
* The application (and possibly the back-end, it's hard to tell) is very unstable. Crashes are a nearly-daily occurrence, and it's not uncommon to try to add something to the calendar only to have it not show up. (Sometimes it never shows up, sometimes it randomly appears later).

I also tried [California][link-8], but this application is very young (version 0.4.0), is as equally unstable as GNOME Calendar, and its future development is in question (it's unclear whether development has ceased or not).

So what am I using currently?

* I try to use GNOME Calendar as much as possible, mostly in a "read-only" fashion to coordinate multiple calendars.
* I use Outlook (either in a local VM or via a hosted desktop) to manage my corporate calendar. It shows up in GNOME Calendar, but GNOME Calendar just isn't ready to use to manage the calendar (lack of timezone support, etc.---all the things listed above).
* I manage my personal CalDAV-based calendars via my mobile devices (either iPhone or Android) or via a web browser. I'm evaluating moving my personal calendars back to Google Calendar, since that is supported by GNOME Calendar.

Note that a number of folks have suggested alternative calendar applications. I've rejected these so far because I don't think they'll fit into my workflow or my environment, but they may work for others. Some of the applications I've seen suggested include [Rainlendar][link-9], [Calcurse][link-10], or [KOrganizer][link-11]. Some of these applications address some of the shortcomings of GNOME Calendar, but none of them address all the major issues I've outlined here (based on my testing thus far).

As you can see, this is most definitely _not_ a solved problem. In fact, if I had to pick one area that is most problematic for me right now, this would be it. Most of the other areas have reasonable solutions---not necessarily ideal solutions, but reasonable solutions. Calendaring and scheduling does not have any reasonable solutions (in my opinion).

In part 3 of the series on corporate collaboration, I tackle the "grab bag" of remaining issues, such as document sharing, meetings/teleconferences, and chat/instant messaging.



[link-1]: https://products.office.com/en-us/business/office
[link-2]: https://www.mozilla.org/en-US/thunderbird/
[link-3]: https://wiki.gnome.org/Apps/Evolution/
[link-4]: https://addons.mozilla.org/en-us/thunderbird/addon/lightning/
[link-5]: https://getfedora.org/
[link-6]: https://wiki.gnome.org/Apps/Calendar
[link-7]: https://products.office.com/en-US/outlook/
[link-8]: https://wiki.gnome.org/Apps/California
[link-9]: http://www.rainlendar.net/cms/index.php
[link-10]: http://calcurse.org/
[link-11]: https://userbase.kde.org/KOrganizer
[link-12]: https://github.com/Ericsson/exchangecalendar/releases
[xref-1]: {{< relref "2017-03-21-linux-migration-corp-collab-pt1.md" >}}
