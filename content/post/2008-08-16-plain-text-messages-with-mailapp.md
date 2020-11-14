---
author: slowe
categories: Rant
comments: true
date: 2008-08-16T09:31:32Z
slug: plain-text-messages-with-mailapp
tags:
- Exchange
- macOS
- Messaging
- Microsoft
title: Plain Text Messages with Mail.app
url: /2008/08/16/plain-text-messages-with-mailapp/
wordpress_id: 825
---

I'm not really sure where or when it started, but over the last couple of years I started taking a strong preference to plain text communications. Perhaps it's an increased amount of time spent on Usenet newsgroups (I'm **still** waiting for Panic to release a substantive update to [Unison](http://www.panic.com/unison/)!), or perhaps its due to the annoyance of HTML e-mail that include more pictures than text; I don't know. In any case, I set my e-mail client (Mac OS X's Mail.app) to use plain text by default when composing messages, and I used the "hidden" preference to show the plain text alternative for messages when it's available:

	defaults write com.apple.mail PreferPlainText -bool TRUE

So that's all well and good, but what I've noticed is that Mail.app seems to "ignore" some of the line endings in my message. It primarily only happens in signatures; I haven't noticed it happening in the body of the message. At the same time that I adopted plain text messages, I also adopted the "standard" signature delimiter of two dashes and a space, so my signature will typically look something like this:

	--  (hidden space at the end)  
	Scott

What happens is that Mail.app turns it into this:

	-- Scott

What in the world? Why is Mail.app playing with my signature? I've also noticed that in my longer signature---where I include my official title, phone numbers, company name, etc.---that Mail.app plays with the line endings there as well.

It also seems that this may be somehow related to Exchange Server 2007, as it only seems to happen to messages sent through my corporate Exchange infrastructure (I use IMAP and SMTP for connectivity to Exchange). I can't find a single instance of an e-mail message where this has happened with any of my other non-Exchange e-mail accounts. But this doesn't really make much sense, because the message I'm seeing is the local copy after it is submitted via SMTP. Perhaps the way in which Mail.app interacts with the SMTP server affects how the message in the Sent mailbox looks? I don't know.

This is _really_ irritating. If I type something, Mail.app (or Exchange Server) should **NOT** be going back and changing what I type. Anyone have any clue what could be going on here, or how I might fix it?
