---
author: slowe
categories: Information
comments: true
date: 2019-05-11T19:00:00Z
tags:
- Linux
- Fedora
- macOS
- Markdown
title: 'The Linux Migration: Preparing for the Migration'
url: /2019/05/11/linux-migration-preparing-for-the-migration/
---

As far back as 2012, I was [already thinking][xref-1] about migrating away from Mac OS X (now known as macOS). While the migration didn't [start in earnest until late 2016][xref-2], a fair amount of work happened in advance of the migration. Since I've had a number of folks ask me about migrating to Linux, I thought I'd supplement [my Linux migration series][xref-3] with a "prequel" about some of the work that happened to prepare for the migration.<!--more-->

In the end---and I imagine some folks may get upset or offended at this---an operating system (OS) is really just a vehicle to deliver applications to the user. While users like myself have strong preferences about their OS and how their OS works, ultimately it is the ability to "get things done" that really matters. This is why I ended up suspending my Linux migration in August 2017; I didn't have access to the applications I needed in order to do what I needed to do. (Though, to be fair, part of that was a lack of growth on my part, though that's a different blog post for a different day.)

To that end, most of the work I did in advance of the migration involved three key areas:

* Understanding the tasks I needed to do and the workflows used to do them
* Mapping my tasks to applications
* Freeing data from proprietary formats

Let's look at each of these individually. Keep in mind there's a pretty fair amount of "fluidity" among these three areas; each area is both informed by and informs the other areas.

## Understanding my Tasks and Workflows

In order to know if you'll be able to do what you need to do using Linux (or Windows, or macOS; it applies equally regardless of your destination OS), you first need to fully understand what you need to do. I know this sounds simplistic---"How could I _not_ know what I need to do?"---but it can be easy to overlook things when you've gotten into a comfortable workflow. On three separate occasions between 2012 (when I first started considering migrating away from macOS) and 2016 (when I actually kicked off a migration), I thoroughly examined the "things I do" (my tasks) as well as how I did those things (my workflows). These examinations exposed potential areas of concern, such as the use of OS-specific scripting functionality (AppleScript) or areas where I had not yet identified a replacement application (see the next section).

I also took this time to identify "critical" tasks---what are the things that I _absolutely_ needed to be able to do, and what are the things it would merely be nice to be able to do?

If you're considering a migration and haven't yet done something like this, I'd _strongly_ urge you to run through this exercise first. When you do, you'll also want to consider any tasks that are being done "automatically" for you. For example, when I was on macOS I used a tool called [Hazel][link-1] to automatically organize files in specific folders. This was a task I needed to do, but which was being done automatically. By making sure to include such tasks, I was able to have a clearer view of what life would be like after the migration.

## Mapping Tasks to Applications

Once you have a good understanding of the things you need to do, then it's time to determine if that functionality exists on your target platform (Linux, in my case). So, in addition to examining my tasks and workflows on multiple occasions, I also went through _every single application_ that was installed on my systems to determine if a Linux equivalent or replacement existed. Early in the timeline (in 2013), I hadn't yet identified replacement applications for many of my macOS applications. As time progressed and I spent more time researching, I identified more and more replacement applications (or I migrated away from such applications; see the next section). In some cases, I realized I didn't use that application very much, and so I stopped using it entirely because my task analysis (what I described in the previous section) indicated that it wasn't needed for any critical tasks. In other cases, I migrated to software-as-a-service (SaaS) alternatives when an equivalent Linux application wasn't available.

This step---and the next step regarding data formats---are likely going to be fairly time-consuming, and you'll very likely need to iterate a couple of times through the process. Of course, it's probable that a fair amount of this will be driven by your job and your employer, who will dictate certain requirements/standards. Be sure to keep that in mind.

## Freeing Data from Proprietary Formats

Proprietary data formats were on my mind [as far back as 2011][xref-4], in advance of my first published mention of migrating away from macOS. It was around that time that I started strongly embracing [Markdown][link-2] and more platform-independent file formats. In looking back over it now, I realize that my embrace of Markdown led to other notable changes in my workflows and applications:

* The [blog migration from WordPress to Jekyll/GitHub Pages][xref-5] was driven largely by my desire to use Markdown for blogging (as well as a desire to more fully include `git` in my regular workflows). It also had the nice effect of freeing me from a macOS-specific application I'd been using to make blogging easier, and unlocking my blog content into a more portable format.
* My use of Markdown for presentations (first via Deckset as described [here][xref-6], later via Remark.js as described [here][xref-7]) helped lessen my reliance on proprietary presentation applications. I moved _all_ my personal presentations to Markdown (well, variants of Markdown).

It wasn't just about Markdown, though Markdown was helpful and played a large role. I had to identify _every_ instance where data was being stored in some proprietary, non-portable format and use that information hand-in-hand with the application research to determine the best course of action. In some cases, there was no migration path. In other cases, I could use an intermediate format that served as a "bridge" between the old application and the new application. In yet other cases, I could export proprietary data formats to a standard-based format (often text-based, like OPML or XML) that I could open and manipulate with a text editor (which had now become perhaps the most important application I used). The ideal scenario is when the new application supported importing the data formats of the old application (support in [LibreOffice][link-3] for working with Microsoft Office documents is a great example).

Those of you considering a migration to a new OS will need to undergo a similar process. I hope that those of you thinking about migrating to Linux find the information I've shared here and in the rest of [the Linux migration series][xref-3] helpful and informative.

## Wrapping Up

The gap of four years between when I first mentioned switching away from macOS and when I actually started a migration in earnest is due partially to a lack of urgency on my part, but there _was_ stuff happening in that time. As I've described in the previous sections, I did spend quite a bit of time thoroughly exploring how I was spending my time, the workflows I was using, the applications I was running, and the data formats that were in use. Armed with that knowledge, I was then able to identify a path forward---either identifying a replacement application and/or replacement data format, migrating to a different application or different data format, or completely retiring applications and data formats that were no longer essential for me. Could I have done this in less time? Oh, most certainly! My point is, though, that anyone considering switching to Linux---or to any alternate OS---is going to need to go through a similar set of exercises in order for their migration to also be successful.

If you're thinking of migrating to Linux, I'd love to hear from you and possibly help answer any questions you may have. Feel free to [contact me on Twitter][link-99]!

[link-1]: https://www.noodlesoft.com/
[link-2]: https://daringfireball.net/projects/markdown/
[link-3]: https://www.libreoffice.org/
[link-99]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2012-11-30-why-i-might-leave-os-x.md" >}}
[xref-2]: {{< relref "2016-12-16-linux-migration-initial-progress-report.md" >}}
[xref-3]: {{< relref "2018-12-23-the-linux-migration-series.md" >}}
[xref-4]: {{< relref "2011-09-07-switching-to-eaglefiler.md" >}}
[xref-5]: {{< relref "2014-12-18-blog-migration-in-the-works.md" >}}
[xref-6]: {{< relref "2015-02-18-presentations-markdown-deckset.md" >}}
[xref-7]: {{< relref "2017-03-07-linux-migration-creating-presentations.md" >}}
