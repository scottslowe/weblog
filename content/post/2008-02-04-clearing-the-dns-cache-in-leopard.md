---
author: slowe
categories: Tutorial
comments: true
date: 2008-02-04T16:13:41Z
slug: clearing-the-dns-cache-in-leopard
tags:
- macOS
- Networking
title: Clearing the DNS Cache in Leopard
url: /2008/02/04/clearing-the-dns-cache-in-leopard/
wordpress_id: 622
---

In previous versions of Mac OS X, I could use this command to flush the DNS resolver cache:

	lookupd -flushcache

However, in Mac OS X version 10.5 "Leopard," this command no longer works! I had an occasion today where I needed to flush the DNS resolver cache due to a configuration error in the lab. Fortunately a quick Google search turned up [this page](http://www.hongkiat.com/blog/how-to-clear-dns-cache-in-mac-osx-leopard/), where I found that the correct command to use in Leopard is this one:

	dscacheutil -flushcache

Works like a champ!
