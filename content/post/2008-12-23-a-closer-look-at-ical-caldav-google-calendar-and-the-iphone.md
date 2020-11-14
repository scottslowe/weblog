---
author: slowe
categories: Explanation
comments: true
date: 2008-12-23T10:24:44Z
slug: a-closer-look-at-ical-caldav-google-calendar-and-the-iphone
tags:
- iPhone
- macOS
- Web
- Collaboration
title: A Closer Look at iCal, CalDAV, Google Calendar, and the iPhone
url: /2008/12/23/a-closer-look-at-ical-caldav-google-calendar-and-the-iphone/
wordpress_id: 1109
---

A few weeks ago I posted an article titled [Manually Configuring iCal for Google Calendar and CalDAV][1], in which I provided a way to configure iCal in Leopard to use CalDAV to communicate with Google Calendar. I'd done this once before, but when Google made the CalDAV support "official" they removed the how-to pages and instead pointed everyone to their application to do it automatically. In any case, a bit of experimentation turned up the right settings, so in the event you're interested, have a look there.

Since that time, I've been taking a closer look at the integration points between iCal, Google Calendar via CalDAV, and the iPhone. What I've found indicates that this may not be the optimal solution _for me_---but it may be just fine for you. Note that this has not deterred me from moving forward with greater use of Google Calendar, it has just shifted my strategy away from the built-in CalDAV support.

Some of the key limitations that I've encountered:

* The "one-calendar-per-account" limitation is probably already well-known and isn't a significant limitation, but one to consider nevertheless. This will mean more space required for your calendar list in the event you want to use multiple calendars via CalDAV. For those that aren't familiar with what I'm talking about, the basic gist of the idea is that when using CalDAV with Google Calendar, each CalDAV account is only allowed to have a single calendar. So, to use multiple calendars, you'll need multiple CalDAV accounts. These can all be the same Google account, but in iCal they'll need to be configured as separate CalDAV accounts.

* The big limitation, for me at least, is that CalDAV calendars synced to the iPhone are, in fact, read-only. That's right---you won't be able to make any changes to such calendars from the iPhone itself. I was pleasantly surprised to see that the calendars did actually sync to the iPhone, and then unpleasantly surprised to find they were now read-only. For me and how I work, I need the ability to make changes to my calendar from my iPhone. If that's not a big deal for you, then continuing down the CalDAV route may be OK. Unfortunately, it's not for me.

* You can't move events between CalDAV-enabled calendars. You actually have to re-create the event on the other calendar, then delete it from the first.

I'm still moving ahead with a greater usage of Google Calendar, but as a result of finding these limitations, I'm now taking a closer look at some of the third-party utilities to provide two-way sync between iCal and Google Calendar. [BusySync](http://www.busymac.com/) is the leading candidate right now, although I'm also looking at [Calgoo Connect](http://www.calgoo.com/connect/index.do) and [Spanning Sync](http://spanningsync.com/). If any readers are using any of these products right now, I'd certainly welcome any feedback on how well they work.

[1]: {{< relref "2008-12-02-manually-configuring-ical-for-google-calendar-and-caldav.md" >}}
