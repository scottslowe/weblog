---
author: slowe
categories: Information
comments: false
date: 2005-08-08T20:56:53Z
slug: internal-news-server-up-and-running
tags:
- OSS
- Collaboration
title: Internal News Server Up and Running
url: /2005/08/08/internal-news-server-up-and-running/
wordpress_id: 70
---

I finally managed to get an internal news server running [INN](http://www.isc.org/index.pl?/sw/inn/) 2.3.5 up and running, and transferring data from the proprietary platform that is currently hosting some internal newsgroups. I decided to use my first real installation of [CentOS](http://www.centos.org/) for the internal news server, and so far it has worked out well.

I have a newsfeed to transfer new postings from the proprietary application over to INN, and I'm using pullnews right now as I write this to transfer the old articles over. Aside from some limitations and failures which I believe to be the result of the proprietary application's specific implementation of NNTP (which I, personally, feel is another [nonstandard implementation][1]), everything seems to be transferring over rather well. The next step will be to add authentication to the INN installation, and then add SSL support.

One more nail in the coffin of a certain proprietary groupware application...


[1]: {{< relref "2005-05-14-nonstandard-implementations.md" >}}
