---
author: slowe
categories: Information
comments: false
date: 2006-03-07T19:48:44Z
slug: mac-os-security-flaw-persists
tags:
- Apple
- Macintosh
- Security
- Web
title: Mac OS Security Flaw Persists
url: /2006/03/07/mac-os-security-flaw-persists/
wordpress_id: 195
---

The [extremely critical security vulnerability](http://secunia.com/advisories/18963) released by [Secunia](http://secunia.com/) a couple of weeks ago was apparently not fixed with the [security update](http://docs.info.apple.com/article.html?artnum=303382) released by [Apple](http://www.apple.com/) last week. According to researchers, it is still possible to disguise an executable as a picture or other type of document.

Security experts were [cited by ZDNet UK](http://news.zdnet.co.uk/software/mac/0,39020393,39256044,00.htm) saying that the underlying problem still existed:

>But Apple failed to address a key part of the problem, the fix should be at a lower, operating system level, experts said. It is now still possible for hackers to construct a file that appears to be a safe file type, such as an image or movie, but is actually an application, they said.

As a result, it is still possible to disguise a file as an innocent type while the file is actually executable. This kind of security problem can be compared to Windows' double-extension problem, which led to such wonderful exploits as the Melissa virus.

I'll say again [what I've said before][1]: No operating system is immune to these kinds of security problems. I'm very confident that regardless of operating system---Windows, Mac OS X, or Linux---any unsuspecting user can be tricked into executing untrusted code. In the end, it all comes down to educating the users. Unfortunately, Mac users have been so indoctrinated that the Macintosh is immune to viruses and security problems that the user base in now facing an uphill climb. In that regard, at least, Windows users have a plus. (I suspect that as more and more non-technically savvy users start using Linux that the same problem will crop up there as well.)

[1]: {{< relref "2006-02-21-mac-users-must-be-careful-too.md" >}}
