---
author: slowe
categories: News
comments: false
date: 2006-05-11T23:04:59Z
slug: may-security-vulnerabilities
tags:
- Exchange
- Microsoft
- Security
- Windows
title: May Security Vulnerabilities
url: /2006/05/11/may-security-vulnerabilities/
wordpress_id: 246
---

Earlier this week, [Microsoft](http://www.microsoft.com/) released a couple of patches on its standard monthly schedule. These patches are designed to plug a couple of critical security flaws, including what appears to be a very serious problem with Microsoft [Exchange Server](http://www.microsoft.com/exchange/).

The two Windows flaws are not terribly serious, in my opinion. One, [MS06-020](http://www.microsoft.com/technet/security/Bulletin/MS06-020.mspx), is rated "Critical" and plugs a problem with the Flash player. So, technically, this isn't a problem with Windows but with Flash, and [Adobe has also released a security bulletin](http://www.adobe.com/devnet/security/security_zone/apsb06-03.html) as well. The second, [MS06-018](http://www.microsoft.com/technet/security/bulletin/ms06-018.mspx), fixes a flaw with the Distributed Transaction Coordinator (DTC). This flaw can only cause a Denial of Service (DoS) condition and can be blocked at perimeter firewalls (but this, of course, won't protect against internal threats).

Other related security advisories:
Secunia: [Microsoft Distributed Transaction Coordinator Two Vulnerabilities](http://secunia.com/advisories/20000/)
Secunia: [Microsoft Windows Flash Player Code Execution Vulnerabilities](http://secunia.com/advisories/20045/)

However, it is the Microsoft Exchange Server vulnerability, [MS06-019](http://www.microsoft.com/technet/security/bulletin/ms06-019.mspx), that is more troubling. Remotely exploitable via anonymous connections (such as SMTP), this exploit is [ripe for an automated worm](http://www.darkreading.com/document.asp?doc_id=94637&f_src=darkreading_section_318). What's worse, typical perimeter firewall protections won't help and no user intervention is required. Simply getting spammed may be sufficient to affect your server! This is one patch to get installed as quickly as possible (after appropriate testing has occurred, of course).

Read the [Secunia advisory on the Exchange flaw](http://secunia.com/advisories/20029/) here.

Also, a third party has [uncovered an additional flaw](http://secunia.com/advisories/20061/) in Windows that has not yet been patched. This vulnerability affects compiled Help files (see more detailed information). This one requires user intervention, so isn't quite as likely to spread via a worm.
