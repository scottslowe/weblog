---
author: slowe
categories: Information
comments: true
date: 2008-12-12T12:05:03Z
slug: google-apps-doesnt-play-nice-with-mailtags
tags:
- macOS
- Messaging
- Web
title: Google Apps Doesn't Play Nice with MailTags
url: /2008/12/12/google-apps-doesnt-play-nice-with-mailtags/
wordpress_id: 1092
---

One of my most used and most useful applications (add-ons?) is [MailTags](http://indev.ca/MailTags.html), by Scott Morrison. It's an add-on to Mail.app that allows for tagging e-mail messages with keywords, projects, notes, etc., and then saving that information in IMAP headers---if you are so inclined---so that it persists from one IMAP client to another. It's a fantastic add-on. I highly recommend it.

Unfortunately, I just discovered a strange interaction between MailTags and Google Apps. I recently moved one of my older e-mail domains over to Google Apps and discovered that every time I try to save MailTags data to IMAP, the message(s) disappear from my Inbox. The only way to recover the messages is to login via the web and recover them. The only workaround is to _not_ save MailTags data back to the IMAP server. This is not an issue with MailTags, by the way; this is a strange interaction that results from the odd way in which Google provides IMAP access to mail data.

Had I bothered to do a bit of research before migrating this domain over to Google Apps, I would have found that this was a known issue and is well-documented on the MailTags forums (see [here](http://www.indev.ca/forum/viewtopic.php?f=10&t=1316) and [here](http://www.indev.ca/forum/viewtopic.php?f=13&t=1117), for example). There does not appear to be a timeframe for a workaround, so for the time being I'll just have to refrain from trying to save MailTags data to IMAP for this particular e-mail account.

Since this is an older domain that isn't mission-critical, this isn't a huge worry. Further, since I only use a single IMAP client and typically don't save a lot of data on the IMAP server, the impact is lessened even more. However, I had been considering moving e-mail for scottlowe.org over to Google Apps as well, and along with that migration I had considered moving more of my e-mail into Google Apps and just use IMAP to keep my client in sync with the server. This would, for example, provide me with greater access to my archived e-mail via my iPhone. With the discovery of this odd behavior, now, I'll have to reconsider that plan. Anyone have any suggestions?
