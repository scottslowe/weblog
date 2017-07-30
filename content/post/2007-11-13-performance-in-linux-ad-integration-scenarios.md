---
author: slowe
categories: Information
comments: true
date: 2007-11-13T16:27:23Z
slug: performance-in-linux-ad-integration-scenarios
tags:
- ActiveDirectory
- Interoperability
- LDAP
- Linux
title: Performance in Linux-AD Integration Scenarios
url: /2007/11/13/performance-in-linux-ad-integration-scenarios/
wordpress_id: 575
---

A reader kindly shared with me some tips and tweaks he used to help resolve some performance issues with Linux-AD integration, and now I'd like to share them with you:

>I ran into some nagging performance issues with Linux/AD integration the other day, and managed to solve them (mostly)---I thought you might be interested in the solution.  
>
>Since I'm not exactly following your integration guide (I use DNS lookups to locate LDAP servers in AD, and use GSSAPI authentication for nss_ldap instead of a binddn), I have a bit more overhead on my getent system calls, that was starting to get noticable when it came to getting directory listings, etc.  
>
>Two things I have done to alleviate this issue:  
>
>1. Since we have a flat catalog, and all of our DCs are also GCs, I
disabled recursion.  This is not universally relevant, but it does assist me
>
>2. I edited `/etc/nscd.conf` to set a 5 minute cache time for passwd and group (but not host!) entries.  Data is still usually piping fresh, but now we only call nss_ldap once every 5 minutes, instead of every time we need to know who owns a file.  Then I started nscd, and set it to start on boot.
>
>NSCD is a standard part of most RHEL and derivative installs.  
>
>Since implementing the above, the user-level experience is no longer
discernably different than the old `/etc/passwd` method of authentication/authorization.

This is good information to have. Thanks to Brandon for sharing his experience!
