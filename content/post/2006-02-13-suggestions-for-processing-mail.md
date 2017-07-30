---
author: slowe
categories: Musing
comments: false
date: 2006-02-13T19:43:10Z
slug: suggestions-for-processing-mail
tags:
- Messaging
- Networking
- OSS
title: Suggestions for Processing Mail
url: /2006/02/13/suggestions-for-processing-mail/
wordpress_id: 178
---

I need some help with a solution for processing mail messages. Specifically, I'm looking for an open source solution that will allow me to create filters (or rules, or policies, or whatever term you'd like to insert here) that will perform actions on inbound mail messages. Does anyone out there have any suggestions?

In case you're wondering why, I need something that can alert me (via e-mail, of course) when an e-mail message with specific words in the subject or body are delivered to a generic mailbox created on the mail server. See, I have multiple customers whose systems and applications send non-urgent reports and notifications to a generic mailbox. When one of those messages contains text that might indicate a failure or a problem, I want to be notified via e-mail on my primary e-mail account. This way, I don't have to check this generic mailbox every day, but instead I can be notified when a failure/error notification arrives.

This solution should integrate with both [Postfix](http://www.postfix.org/) and [Dovecot](http://www.dovecot.org/), as I use currently use both of these. I've looked at [procmail](http://www.procmail.org/) and [maildrop](http://www.courier-mta.org/maildrop/), and these both seem good, but which is better? Which is faster, more efficient, more flexible? This is where I could use some feedback from those of you out there that have used these programs before, and can provide real-world input. By the way, I'm using Maildirs instead of standard mailbox files, so I'll need something that can work with Maildirs.

So, anyone have any suggestions for me? Or, at the very least, some links to clear, concise instructions for either procmail or maildrop? Feel free to bookmark them via [del.icio.us](http://del.icio.us/) and tag them as "for:slowe", if you are so inclined. Thanks!
