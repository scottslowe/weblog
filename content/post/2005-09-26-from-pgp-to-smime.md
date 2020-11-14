---
author: slowe
categories: Rant
comments: false
date: 2005-09-26T16:40:32Z
slug: from-pgp-to-smime
tags:
- Encryption
- macOS
- Security
title: From PGP to S/MIME
url: /2005/09/26/from-pgp-to-smime/
wordpress_id: 95
---

Not long after switching to [Mac OS X](http://www.apple.com/macosx/), I also started using [PGP](http://www.pgp.com/) to provide a digital signature to all my business-related electronic communications. Given the increasing frequency of e-mail messages with spoofed source addresses, I felt that it was only prudent to start providing customers with a way of proving that messages which stated they were from me were actually from me. Besides, that might also cause some customers to ask, "Is this something I should be considering as well?"

Since that time, I've been reasonably pleased with PGP. However, I recently started looking at a possible upgrade to the latest version of Mac OS X ("Tiger"), and noticed that PGP 8.x is not compatible. In fact, PGP stated rather clearly that PGP 8.x is not and would not be compatible with Tiger or future releases of Mac OS X. (This statement was located on the [PGP Support](http://www.pgpsupport.com/) web site.)

I've researched PGP 9.x and read some reviews, and I'm not terribly excited about the new architecture. I'd much rather leave my existing SSL/TLS configuration in place and **_know_** that my messages are encrypted when going to/from my server, rather than relying on a PGP "proxy" to enforce policies. I suppose it's a good way of handling things, but when it comes down to it I just don't like it.

That being said, in order to proceed with an upgrade to Tiger I'll have to a) upgrade to PGP 9.x; b) switch to an open source implementation of PGP, such as [GPG](http://www.gnupg.org/); c) switch to an alternate form of digital signatures, such as [S/MIME](http://en.wikipedia.org/wiki/S/MIME); or d) stop using any form of digital signing whatsoever. Having purchased PGP in order to not use GPG, I'd say that option is out. Not having any signatures whatsoever isn't really acceptable to me either, so that leaves S/MIME.

S/MIME is fine by me, anyway, since [Microsoft](http://www.microsoft.com/) has done a reasonably good job of incorporating S/MIME support into recent versions of [Outlook](http://www.microsoft.com/office/outlook/). Since a significant portion of my customers and colleagues use Outlook, then it begins to make sense to use S/MIME.

Over the next few weeks, I'm going to be researching which certificate authority (CA) I'll use to issue my S/MIME certificates. I've registered with [CAcert.org](http://www.cacert.org/), in the event that I decide to use them. My preference would be to use a CA that can offer me a certificate with my actual name on it. On the other hand, I'm cheap (rather, I'm frugal) and I don't feel like paying $20/yr just to maintain a certificate. It may be that in order to get my actual name on the certificate I'll have to pay for it.

I've already located a number of resources in using S/MIME with Mac OS X 10.3 ("Panther"), my current version, and those have been added to [my del.icio.us](http://del.icio.us/slowe) bookmark list (usually with the [Encryption](http://del.icio.us/slowe/Encryption) or [Security](http://del.icio.us/slowe/Security) tags). If anyone has any other resources that may be helpful, feel free to let me know.
