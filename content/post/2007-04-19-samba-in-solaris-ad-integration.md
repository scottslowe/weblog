---
author: slowe
categories: Information
comments: true
date: 2007-04-19T13:20:04Z
slug: samba-in-solaris-ad-integration
tags:
- ActiveDirectory
- Interoperability
- Samba
- Solaris
- UNIX
title: Samba in Solaris-AD Integration
url: /2007/04/19/samba-in-solaris-ad-integration/
wordpress_id: 444
---

Using [Samba](http://www.samba.org/) in Linux-AD integration scenarios is tremendously helpful because it removes the need to manually create the SPNs and export the keytabs out of Active Directory. I wrote up my first test of [Samba in Linux-AD integration][1], then proceeded to verify that procedure and include it in the full [Linux-AD integration instructions][2]. But would it work in Solaris-AD integration scenarios?

In theory, yes. After all, Samba is included with Solaris 10, right? Right! Unfortunately, the version of Samba that's included was not compiled with ADS support, and recompiling Samba to include ADS support means also recompiling a whole ton of other junk as well. Fortunately, [this thread](http://forum.java.sun.com/thread.jspa?threadID=5154771) titled "Samba on Solaris 10 in native ADS environment" gives complete instructions on how to compile Samba with ADS support on Solaris 10 so that one _can_ use Samba for AD integration.

I plan to test this in my lab as soon as possible, and when I have some first results I'll post more information here.

[1]: {{< relref "2006-12-19-using-samba-in-linux-ad-integration.md" >}}
[2]: {{< relref "2007-01-15-linux-ad-integration-version-4.md" >}}
