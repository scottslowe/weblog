---
author: slowe
categories: Information
comments: false
date: 2005-06-14T10:42:41Z
slug: new-delicious-links
tags:
- Apache
- OSS
- Security
- Web
title: New del.icio.us Links
url: /2005/06/14/new-delicious-links/
wordpress_id: 14
---

I've added a couple new links to [my del.icio.us](http://del.icio.us/slowe/) bookmarks. Most notably, I've added an entry for a recent article on [ONLamp.com](http://www.onlamp.com/), a site hosted by the [O'Reilly Network](http://www.oreillynet.com/), on [mod_security](http://www.modsecurity.org/).

The article, [Securing Web Services with mod_security](http://www.onlamp.com/pub/a/onlamp/2005/06/09/wss_security.html?page=1), is a great introduction to an [Apache](http://httpd.apache.org/) module that I've known about and used for a while now. Similar to [URLScan](http://www.microsoft.com/technet/security/tools/urlscan.mspx) for IIS 5.0, mod_security serves to block malicious URLs and invalid requests, even within encrypted HTTPS sessions. As such, it acts as the perfect complement to firewalls and intrusion detection/prevention systems (both network-based and host-based), which typically operate at lower layers of the networking stack.

If you use Apache, I urge you to evaluate and deploy mod_security to enhance your web server's security.
