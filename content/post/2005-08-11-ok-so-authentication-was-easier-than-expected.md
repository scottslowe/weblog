---
author: slowe
categories: Information
comments: false
date: 2005-08-11T18:10:55Z
slug: ok-so-authentication-was-easier-than-expected
tags:
- Collaboration
- OSS
title: OK, So Authentication Was Easier Than Expected...
url: /2005/08/11/ok-so-authentication-was-easier-than-expected/
wordpress_id: 72
---

...but SSL is not so easy. I found a workaround for using [Stunnel](http://stunnel.mirt.net/index.html); in order for [INN](http://www.isc.org/products/INN/) to not think it's another news server feeding it information and instead treat it like a reader, I had to alias another IP address and bind Stunnel on that IP address. It works, but it's not my ideal solution.

To further complicate matters, it looks like I'll have to compile INN from source myself in order to get SSL support in nnrpd (that's the component that handles connections from NNTP readers). It's not a big deal, I know. It's just my history compiling from source has been a bit rocky.
