---
author: slowe
categories: News
comments: true
date: 2007-02-06T12:02:38Z
slug: new-excel-zero-day-attack
tags:
- macOS
- Microsoft
- Office
- Security
- Windows
title: New Excel Zero-Day Attack
url: /2007/02/06/new-excel-zero-day-attack/
wordpress_id: 409
---

This past Friday, the Microsoft Security Response Center blog [posted a notification](http://blogs.technet.com/msrc/archive/2007/02/02/microsoft-security-advisory-932553-posted.aspx) about [Microsoft Security Advisory 932553](http://www.microsoft.com/technet/security/advisory/932553.mspx), which describes the specific issue and the attacks around that issue.

More information on the issue is also available from this [Secunia advisory](http://secunia.com/advisories/24008) and from [US-CERT](http://www.kb.cert.org/vuls/id/613740).

There are two interesting things to note (interesting to me, at least):

* First, this is an _Office_ vulnerability, not a _Windows_ vulnerability. Therefore, as correctly pointed out in the security advisories, Office 2004 for the Mac is also affected.

* Second, although the current attacks are targeted against Excel, this vulnerability extends to all Office documents. This means that other forms of attack could be forthcoming in the near future until the underlying flaw is addressed.

As with some of the other zero-day attacks I've discussed here, it looks like the only workaround available at this time is to not open Office documents from untrusted sources. In fact, it would probably be best not to open any unexpected and/or unsolicited Office document from any source, trusted or otherwise.

Other related links:  

[MS warns of Excel 'zero-day' attack - MacNN](http://www.macnn.com/articles/07/02/05/excel.zero.day.attack/)  

[New Zero-Day Threat Excels - eWeek](http://www.eweek.com/article2/0,1759,2090525,00.asp)
