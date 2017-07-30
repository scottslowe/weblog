---
author: slowe
categories: Information
comments: false
date: 2005-05-15T16:31:02Z
slug: perdition-working-now
tags:
- Encryption
- Exchange
- IMAP
- Messaging
- Microsoft
- OSS
- Security
- SSL
title: Perdition Working Now
url: /2005/05/15/perdition-working-now/
wordpress_id: 6
---

I finally managed to get [Perdition](http://www.vergenet.net/linux/perdition/) working. Still unable to confirm if [Mac OS X's](http://www.apple.com/macosx/) Mail.app supports STARTTLS (my experience thus far says No), I had to resort to using [Stunnel](http://stunnel.mirt.net/index.html) to wrap IMAP inside an SSL tunnel, then forward the IMAP traffic to Perdition on the same host. The Perdition proxy then passes the traffic to the back-end mail server. It's not the solution that I really wanted, but it will do for now. At least the [Exchange Server 2003](http://www.microsoft.com/exchange/) IMAP server isn't exposed directly to external networks.

On a slightly related note, the [Slipstick Systems](http://www.slipstick.com/) web site has a link to an IMAP proxy server that implements STARTTLS as a workaround for Exchange's lack of native support for STARTTLS. The IMAP proxy can be found at [http://www.slipstick.com/files/imapproxysvc.zip](http://www.slipstick.com/files/imapproxysvc.zip). So, if you have an IMAP4 client that supports STARTTLS and want to connect it to Exchange, you can use this IMAP proxy. At least, until [Microsoft](http://www.microsoft.com/) puts STARTTLS support into Exchange directly.
