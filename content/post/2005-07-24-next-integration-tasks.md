---
author: slowe
categories: Information
comments: false
date: 2005-07-24T23:58:13Z
slug: next-integration-tasks
tags:
- Apache
- Cisco
- Kerberos
- Microsoft
- Interoperability
title: Next Integration Task(s)
url: /2005/07/24/next-integration-tasks/
wordpress_id: 57
---

With the majority of my Linux servers now authenticating against Active Directory, I'm now able to broaden my integration focus and work on some related tasks:

* _SASL2/PAM:_ I still have one server, running SASL2, that has not been switched over to the standard Kerberos/LDAP configuration. I'll need to research the interplay between SASL2 and PAM before I tackle this one.

* _OpenBSD Authentication:_ I haven't touched any of the OpenBSD servers yet for Kerberos/LDAP authentication.

* _VPN Authentication via RADIUS:_ I'd like to use RADIUS to handle some VPN authentication against Active Directory as well. I don't anticipate this should be too terribly difficult, but it is something that is rather new to me.

* _Apache Authentication via Kerberos to AD:_ One of the documents that helped me in getting the pam_krb5 stuff working was for [using mod_auth_kerb with Apache](http://www.grolmsnet.de/kerbtut/) (more information also posted [here](http://sl.mvps.org/docs/LinuxApacheKerberosAD.htm) as well). I'd like to deploy this for some select areas of our intranet and extranet sites, to add an additional layer of security on top of what is already present.

Of course, this is in addition to trying to establish an internal news server running INNd (and then migrating content from Exchange Server 2003 into this news server) and working on Squid log analysis tools. I'll probably start investigating Squid authentication options as well, since that would be very helpful to my customers (especially if I can get the authentication to be transparent, or very nearly so). On top of that I have duties in church, work as a manager overseeing employees, and things to do as a dad. Whew! I often wonder if I am just not efficient with managing my time, or if I just have too much to do.
