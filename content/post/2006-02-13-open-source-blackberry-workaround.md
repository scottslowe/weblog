---
author: slowe
categories: Information
comments: false
date: 2006-02-13T16:05:20Z
slug: open-source-blackberry-workaround
tags:
- Messaging
- Mobility
- OSS
- SSL
- Treo
title: Open Source Blackberry Workaround?
url: /2006/02/13/open-source-blackberry-workaround/
wordpress_id: 177
---

[Funambol](http://www.funambol.com/), formerly Sync4j, is claiming that its latest product, Funambol 3, could function as an open-source Blackberry workaround, allowing push e-mail to be delivered to a variety of mobile devices, including Blackberries. This is particularly important in light of the threat of a Blackberry shutdown due to the [RIM-NTP patent dispute](http://www.eweek.com/article2/0,1759,1917831,00.asp).

Currently in beta, the v3 server software runs on Linux or Windows, and clients are available for Outlook, Windows Mobile, Blackberry, Palm, and (believe it or not) iPod.

More information can be found in [this article](http://www.linux-watch.com/news/NS4243210427.html).

Some people are also suggesting [the use of open standards to ease the impact](http://www.eweek.com/article2/0,1895,1916095,00.asp) of a potential Blackberry shutdown as well. While not as functionally rich as a typical Blackberry implementation, the use of POP3/IMAP4 and SMTP to handle mobile messaging needs is certainly very viable. This is easily implemented via most commercial messaging systems and through a number of open source packages as well. For example, I use [Dovecot](http://dovecot.org/) and [Postfix](http://www.postfix.org/) to provide IMAP4/SMTP support for my Treo, all secured by SSL/TLS encryption. Like the Funambol approach, this also offers a great deal of client-side variety as well, instead of locking users into a single client device.
