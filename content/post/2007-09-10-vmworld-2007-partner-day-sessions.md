---
author: slowe
categories: Liveblog
comments: true
date: 2007-09-10T19:46:51Z
slug: vmworld-2007-partner-day-sessions
tags:
- Backup
- ESX
- VDI
- Virtualization
- VLAN
- VMworld2007
title: VMworld 2007 Partner Day Sessions
url: /2007/09/10/vmworld-2007-partner-day-sessions/
wordpress_id: 531
---

I had the opportunity to attend three Partner Day sessions today, all three from the "Advanced In-Depth Technical Track."

The first was this morning, and it was focused on an in-depth technical review of VMware Consolidated Backup. I've written a few articles about VMware Consolidated Backup before:

* [VMware Consolidated Backup][1]

* [Restoring VCB Full Backups with VMware Converter][2]

* [Strange VCB Error][3]

The presenter was very knowledgeable; in fact, he was the one running the VCB labs at VMworld (and ran the labs at VMworld last year, which I attended). I got some great information from the session, and when I have more time I'll compile that information here. Some of the key points I took away from the session included information on a command-line interface (CLI) for VMware Converter that allows for automated restores of VCB full VM backups using Converter (I'm really excited about looking into that one); good information on the minimal permissions needed for the user that logs into VirtualCenter and whose login information is hard-coded in `config.js` (this is a _big_ security concern for many customers); and some RDM compatibility mode issues (RDM in virtual compatibility mode versus physical compatibility mode). It was a great session; I thoroughly enjoyed it.

Unfortunately, the next session (the first session after lunch) was not nearly as useful. Although included in the same in-depth track, there was a remarkable lack of technical information. Fortunately, it was only 45 minutes long.

The final session of the day was on VMware's VDI and ACE solutions, including a first look at VDM (Virtual Desktop Manager) 2.0, which VMware just [publicly announced earlier today][4]. The presenter, Tommy Walker of VMware, was a great presenter and I enjoyed the presentation. I'm hoping to be able to catch up with Tommy later this week to conduct some in-depth comparisons of VDM with other brokers, such as Leostream (a broker with which I've worked fairly extensively---and which, by the way, [just released](http://www.earthtimes.org/articles/show/news_press_release,174291.shtml) version 5.0 of their broker).

As soon as I have some additional time, I'll try to post some additional information about these sessions and some of the in-depth technical details presented.

[1]: {{< relref "2007-03-05-vmware-consolidated-backup.md" >}}
[2]: {{< relref "2007-03-06-restoring-vcb-full-backups-with-vmware-converter.md" >}}
[3]: {{< relref "2007-08-07-strange-vcb-error.md" >}}
[4]: {{< relref "2007-09-10-vmworld-2007-partner-day.md" >}}
