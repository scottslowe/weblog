---
author: slowe
categories: Information
comments: false
date: 2005-07-17T22:31:52Z
slug: linux-ad-integration-success
tags:
- Interoperability
- ActiveDirectory
- Kerberos
- LDAP
- Linux
- Microsoft
- Windows
title: Linux-AD Integration Success!
url: /2005/07/17/linux-ad-integration-success/
wordpress_id: 51
---

Well, limited success, anyway. I have managed to get Linux authentication to occur via [Kerberos](http://web.mit.edu/kerberos/www) against Active Directory. LDAP is used to lookup the user and group information. Using `pam_krb5`, authentication occurs via Kerberos to an Active Directory DC for any PAM-aware application. I'm sure that I'll find some hiccups along the way, but so far things look good. I only have 1 server configured this way for now (a non-essential server), but after some additional testing I'll expand this to the remainder of my Linux servers.

I'll post more details here and/or on the [Mercurion Systems](http://www.mercurionsystems.com/index.html) web site once all the bugs have been worked out. My thanks go out to the many, many individuals who posted information on using Kerberos and LDAP with Active Directory; this would not have been possible without their assistance.
