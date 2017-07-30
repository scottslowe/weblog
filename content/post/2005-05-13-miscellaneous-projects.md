---
author: slowe
categories: Information
comments: false
date: 2005-05-13T22:22:53Z
slug: miscellaneous-projects
tags:
- Networking
- Messaging
- Microsoft
- Newsgroups
- OSS
- Web
title: Miscellaneous Projects
url: /2005/05/13/miscellaneous-projects/
wordpress_id: 4
---

In my copious spare time (fellow parents out there know this is a joke), I'm working on a couple of miscellaneous technical projects:

* _Internal news server:_ I'm having so many problems with NNTP-based access from [Unison](http://www.panic.com/unison/) to [Exchange Server 2003](http://www.microsoft.com/exchange/) that I've decided to setup an internal NNTP server using Linux and INNd. Linux I'm familiar with; INNd is completely new to me. Wish me luck.

* _IMAP proxy:_ This would use [Perdition](http://www.vergenet.net/linux/perdition/) to proxy IMAP4S requests to my mail server. I'll probably have Perdition handle IMAP4S and then pass IMAP to the back-end server, offloading the SSL work to the proxy.

* _Squid log analysis:_ I'm looking for a way to parse down the logs from my [Squid](http://www.squid-cache.org/) web cache. Once I find a way to do this, then I can really begin offering Squid and [SquidGuard](http://www.squidguard.org/) as a content filtering solution to some of my customers.

I'm sure this stuff is probably old hat to some of you out there. Feel free to contribute some helpful information or URLs.  I'll keep you posted on how things proceed with these and other projects as they develop.
