---
author: slowe
categories: Information
comments: true
date: 2007-04-24T15:17:48Z
slug: solaris-ad-integration-update-coming
tags:
- ActiveDirectory
- Interoperability
- Kerberos
- LDAP
- Microsoft
- Samba
- Solaris
- UNIX
- Windows
title: Solaris-AD Integration Update Coming
url: /2007/04/24/solaris-ad-integration-update-coming/
wordpress_id: 445
---

The last update to the [Solaris 10-Active Directory integration instructions][1] was in October of last year, over six months ago. Since that time, [Sun](http://www.sun.com/) has released another update to [Solaris](http://www.sun.com/software/solaris/) (Solaris 10 11/06, or Update 3) and I have been able to gather some additional information on using an Active Directory-aware version of [Samba](http://www.samba.org/) to help with the process (much like described in the latest version of the [Linux-AD instructions][2]).

The new version will use Kerberos for authentication, LDAP for account information, and Samba to do the "heavy lifting" of joining Active Directory, creating the necessary objects, and creating the keytab and keytab entries on Solaris.

I hope to post the updated integration instructions within the next few days, before I have to leave for a business trip to Canada.

[1]: {{< relref "2006-10-16-refined-solaris-10-ad-integration-instructions.md" >}}
[2]: {{< relref "2007-01-15-linux-ad-integration-version-4.md" >}}
