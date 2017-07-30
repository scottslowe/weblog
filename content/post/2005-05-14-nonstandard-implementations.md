---
author: slowe
categories: Rant
comments: false
date: 2005-05-14T22:24:04Z
slug: nonstandard-implementations
tags:
- Encryption
- Exchange
- IMAP
- Messaging
- Microsoft
- Security
- SSL
- Standards
- Interoperability
title: Nonstandard Implementations
url: /2005/05/14/nonstandard-implementations/
wordpress_id: 5
---

In my experiments with [Perdition](http://www.vergenet.net/linux/perdition/), I learned a couple of very interesting facts. First, the IMAP4 implementation on [Exchange Server 2003](http://www.microsoft.com/exchange/) does not support the STARTTLS command, as described in [RFC 2595](http://www.networksorcery.com/enp/rfc/rfc2595.txt) and re-affirmed in [RFC 3501](http://www.networksorcery.com/enp/rfc/rfc3501.txt). Instead, Exchange expects an SSL session to be established immediately, and then IMAP is spoken. This is similar to the `smtpd_tls_wrappermode` directive that [Postfix](http://www.postfix.org/) supports.

Second, it appears that the [Mac OS X](http://www.apple.com/macosx/) Mail application (commonly referred to as Mail.app) also uses this IMAP-over-SSL approach, since I've been using Mail.app to connect to Exchange using IMAP with SSL for quite some time. I'm trying to confirm that now, but having precious little luck finding any definitive information one way or the other. If anyone knows for certain, please let me know. I'm going to keep searching.

This is one of those things that just makes me crazy.
