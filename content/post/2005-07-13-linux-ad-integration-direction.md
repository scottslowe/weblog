---
author: slowe
categories: Explanation
comments: false
date: 2005-07-13T17:09:06Z
slug: linux-ad-integration-direction
tags:
- Kerberos
- LDAP
- Linux
- Microsoft
- Interoperability
- ActiveDirectory
title: Linux-AD Integration Direction
url: /2005/07/13/linux-ad-integration-direction/
wordpress_id: 47
---

I've spent some time over the last few days researching some of the various ways in which to allow users to login to Linux servers using their credentials from Active Directory. Along the way, I've found some useful articles; notably, [here](http://www.networkcomputing.com/1305/1305ws1.html), [here](http://www.timkennedy.net/docs/Linux+Active_Directory.html), and [here](http://www.newsforge.com/article.pl?sid=04/12/09/2258243). These are also bookmarked in [my del.icio.us](http://del.icio.us/slowe) bookmark list under the [Linux](http://del.icio.us/slowe/Linux) tag.

It looks as if the best solution involves the use of [Kerberos](http://web.mit.edu/kerberos/www) for authentication and LDAP for user/group name resolution. As fate would have it, as soon as I decide to use Kerberos, some [new security holes](http://www.eweek.com/article2/0,1759,1836591,00.asp) are discovered. At least these security holes don't involve Microsoft's implementation of Kerberos, but they will affect the Linux Kerberos clients. Hopefully, patches for the affected portions will be released reasonably quickly.

As this project evolves, I'll continue to post more information here. A final "how to" will likely also be posted on the [Mercurion Systems web site](http://www.mercurionsystems.com/) as well.
