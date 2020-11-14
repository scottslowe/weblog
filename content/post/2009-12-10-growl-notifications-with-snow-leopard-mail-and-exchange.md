---
author: slowe
categories: Information
comments: true
date: 2009-12-10T10:55:50Z
slug: growl-notifications-with-snow-leopard-mail-and-exchange
tags:
- Exchange
- macOS
- Messaging
title: Growl Notifications with Snow Leopard Mail and Exchange
url: /2009/12/10/growl-notifications-with-snow-leopard-mail-and-exchange/
wordpress_id: 1768
---

Like many Mac users, I use [Growl](http://growl.info/) to provide customizable, centralized notifications for events occurring on my system. Rather than use the Growl team's GrowlMail plug-in, I use a custom AppleScript that I wrote that provide new message notifications via Growl. It's not a terribly advanced script; it just provides per-message notifications for each message received, unless I receive more than 5 messages at a time in which case the script just provides a summary notification. It's worked very well for me for quite some time.

Now, following my upgrade to a new MacBook Pro running Snow Leopard, I'm finding that the script has an interesting flaw: when new messages are received via my Exchange account at work, the script notifies me using information from the previous message received on that account rather than the information from the newly-received message. I know that it's not the script because notifications for all other accounts work just fine. Only the Exchange account---which uses Snow Leopard's new Exchange support for connectivity---is affected.

Has anyone else seen this? If so, does anyone have a fix?

**UPDATE:** I wasn't able to make Growl notifications invoked from an AppleScript work properly with my Exchange account, so I tried switching to GrowlMail. Unfortunately, GrowlMail has problems with Snow Leopard 10.6.2 that can only be fixed using the Terminal commands in [this article](http://langui.sh/2009/11/09/fixing-growlmail-letterbox-for-mail-4-2/). After getting GrowlMail recognized by Mail.app, notifications started appearing correctly for all accounts including my Exchange account.
