---
author: slowe
categories: Tutorial
comments: false
date: 2005-10-18T14:42:43Z
slug: protecting-against-openssl-sslv2-flaw
tags:
- Security
- Linux
- Web
title: Protecting Against OpenSSL SSLv2 Flaw
url: /2005/10/18/protecting-against-openssl-sslv2-flaw/
wordpress_id: 104
---

The [recent flaw](http://www.openssl.org/news/secadv_20051011.txt) in [OpenSSL](http://www.openssl.org/) (versions prior to 0.9.7h and 0.9.8a) highlights the fact that SSL is not a security panacea. (You can get more information about this flaw from the link above, from [this _eWeek_ article](http://www.eweek.com/article2/0,1759,1870003,00.asp), or from [this Netcraft post](http://news.netcraft.com/archives/2005/10/11/openssl_patches_security_hole.html).)

Disabling the SSL v2 protocol entirely is another way to protect against this flaw. Since OpenSSL is a set of libraries that other applications use to add SSL/TLS functionality, though, that configuration occurs within the specific applications, not within OpenSSL itself. Here's how to protect [Apache](http://httpd.apache.org/) by disabling SSL v2.

To disable SSL v2 support within Apache 2.0 with mod_ssl, use the following directive in the Apache configuration:

`SSLProtocol +All -SSLv2`

This enables SSLv3 and TLSv1, but disables SSLv2.  (Also see this Apache httpd [documentation](http://httpd.apache.org/docs/2.0/mod/mod_ssl.html#sslprotocol).)

I searched for but couldn't find a similar workaround for [Postfix](http://www.postfix.org/). The `smtpd_tls_cipherlist` and `smtp_tls_cipherlist` directives allow you to specify the supported ciphers and refer you to the OpenSSL documentation. However, the OpenSSL documentation didn't seem to indicate a clear way of disabling only SSL v2. There did appear to be some discussion of adding this functionality in Postfix 2.3. If anyone knows of the particular keywords to specify with the `smtpd_tls_cipherlist` and `smtp_tls_cipherlist` commands for Postfix (or knows of any other way to disable SSL v2 support in Postfix), please let me know.
