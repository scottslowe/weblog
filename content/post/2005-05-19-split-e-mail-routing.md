---
author: slowe
categories: Information
comments: false
date: 2005-05-19T23:59:39Z
slug: split-e-mail-routing
tags:
- Messaging
- Postfix
- Newsgroups
title: Split E-Mail Routing
url: /2005/05/19/split-e-mail-routing/
wordpress_id: 8
---

Now that I have [Perdition](http://www.vergenet.net/linux/perdition/) up and running (although not in the way I really wanted; see my post titled "Perdition Working Now"), I'm moving on to setting up an internal news server.

Before I can get the internal news server up and running, though, I must first address the issue of e-mail submissions to these newsgroups. See, right now I can send an e-mail to _newsgroupname@domain.com_ (this is obviously an invalid address) and that message will be posted to the newsgroup. This works well because the mailboxes and the newsgroups live on the same server and the mail gateway can route all messages to this server.

If I setup a separate news server, however, I'll need some e-mail addresses to be directed to the mail server, but other e-mail addresses (the e-mail addresses for the newsgroups) to a different server altogether. I think that [Postfix](http://www.postfix.org/) can do this, but I don't know that for certain yet. I suspect that the answer lies somewhere in the mystery of virtual_alias_maps, but I just can't wrap my head around it right now. Of course, it _**is**_ getting late here so that may explain it.
