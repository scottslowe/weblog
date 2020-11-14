---
author: slowe
categories: Tutorial
comments: true
date: 2008-12-02T17:57:21Z
slug: manually-configuring-ical-for-google-calendar-and-caldav
tags:
- macOS
- Web
- Collaboration
title: Manually Configuring iCal for Google Calendar and CalDAV
url: /2008/12/02/manually-configuring-ical-for-google-calendar-and-caldav/
wordpress_id: 1078
---

Sometimes I like doing things manually. I can't tell you why, except perhaps to say that I'd rather know exactly what's going on and how things are happening instead of giving over control to a "black box" that does something for me and spits out the results. It's silly, I know, but that's just how I am.

Case in point: today Google announced [official support](http://googlemac.blogspot.com/2008/12/google-calendar-now-supports-apple-ical.html) for CalDAV with Apple iCal. If you've followed my blog for any significant length of time you'll recall that I used Google Calendar back around the VMworld 2008 timeframe, with iCal and CalDAV, to help coordinate vendor meetings and such. So, today, I decided that starting in January, I'd move all my calendaring to Google Calendar and manage it via CalDAV from iCal. So I go out to the Google Help site and look up the instructions for connecting iCal to Google Calendar via CalDAV.

Alas, the old instructions are gone; all that remains are [new instructions](http://www.google.com/support/calendar/bin/answer.py?answer=99358#ical) that say "Use our [new tool](http://code.google.com/p/calaboration/downloads/list)!" Hey, this is a nifty tool and all, but I'd prefer to do it manually. Where's the information on doing it manually? Gone, apparently.

After far too long using Google to search for information on how to manually configure iCal to use CalDAV to talk to Google Calendar---and primarily getting results back that were from Google and had no such information---I finally stumbled across [this page](http://www.marc-seeger.de/2008/07/28/google-calendar-supports-caldav/) that described how to use Google Apps and CalDAV support with iCal. Piecing together bits of information from there and the previous time I configured iCal, I was able to make it work.

For the benefit of everyone else out there who prefers to do things the hard way, here's the information you need to manually configure iCal to use CalDAV with Google Calendar.

1. In iCal, select iCal > Preferences... and then click on Accounts.

2. Click the + sign to add a new account.

3. Specify a description. For username, add your Google Calendar login information, like "username@gmail.com", and put in your password.

4. Expand the "Server Options" section to expose the Account URL setting.

5. For Account URL, specify "https://www.google.com/calendar/dav/**[Google Calendar ID]**/user". Note that it's actually "/user" there at the end, not your user name. The Google Calendar ID in brackets is visible in Google Calendar by going to the settings for a calendar and looking toward the bottom of the Calendar Details tab. You'll see some funky junk like "asdfjklasdfjklasdjklasdjkl@group.calendar.google.com" or similar. That's the Google Calendar ID.

6. Click Add.

That should be it. Note that these instructions work for any calendar you create in Google Calendar, but they don't work for Google Apps users. Google Apps users should follow the link above for information.

By the way, it's also worth mentioning that CalDAV accounts like this are synced to your iPhone, too. Handy.
