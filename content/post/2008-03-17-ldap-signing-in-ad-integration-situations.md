---
author: slowe
categories: Information
comments: true
date: 2008-03-17T13:06:26Z
slug: ldap-signing-in-ad-integration-situations
tags:
- ActiveDirectory
- Interoperability
- Kerberos
- LDAP
- Linux
- Security
title: LDAP Signing in AD Integration Situations
url: /2008/03/17/ldap-signing-in-ad-integration-situations/
wordpress_id: 662
---

Reader Jeffrey Spear contacted me a while back with some problems he was experiencing in trying to integrate some Linux systems into Active Directory. Basically, Kerberos was working but LDAP wasn't. He was able to use `kinit <AD username>` to generate a Kerberos ticket, but using the `getent passwd <AD username>` was not working. No error messages, nothing; it just didn't work.

We traded e-mails back and forth for a while, and eventually he found the solution himself:

>We work with a locked down version of OSs and in this case a domain policy on the Windows server was preventing the RHEL machines from accessing account info. The policy was "Domain controller: LDAP server signing requirements" which was set to "Require signature." When I changed this setting to "None" it worked great.

This is good information and important to keep in mind; I'll be sure to incorporate this into the next revision of the Linux-AD integration instructions. (No, I don't have a timeframe on when that will be!)

In the meantime, if anyone has a workaround for this problem that will allow LDAP to work with signatures enabled or required, I'd love to hear it. Speak up in the comments below!
