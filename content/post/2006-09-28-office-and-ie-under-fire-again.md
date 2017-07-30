---
author: slowe
categories: News
comments: true
date: 2006-09-28T23:25:11Z
slug: office-and-ie-under-fire-again
tags:
- IE
- Office
- Security
title: Office and IE Under Fire (Again)
url: /2006/09/28/office-and-ie-under-fire-again/
wordpress_id: 342
---

News of the [unpatched PowerPoint vulnerability](http://www.eweek.com/article2/0,1759,2021358,00.asp) (via _eWeek_) comes after a summer-long struggle to contain vulnerabilities in [Microsoft Office](http://www.microsoft.com/office/), the office suite that maintains a venerable monopoly in the market. As with previous PowerPoint exploits, this one uses a rigged PowerPoint file to install a backdoor application. I found some additional information available from [Symantec](http://www.symantec.com/); read that [here](http://www.symantec.com/enterprise/security_response/weblog/2006/09/the_microsoft_office_vulnerabi.html).

Similarly, another [exploit has surfaced](http://www.eweek.com/article2/0,1759,2021959,00.asp) for Internet Explorer. This exploit takes advantage of a flaw that was supposedly brought to Microsoft's attention back in July and apparently still remains unpatched. Fortunately, additional information on the IE vulnerability is available; here are some relevant links:

SecurityFocus: [Microsoft Internet Explorer WebViewFolderIcon Buffer Overflow Vulnerability](http://www.securityfocus.com/bid/19030)  
osvdb: [Microsoft IE WebViewFolderIcon setSlice Overflow](http://osvdb.org/27110)

No word yet on any workarounds for this vulnerability or the published exploit.

Finally, in slightly related news...a couple of days ago Microsoft released an [out-of-band patch (MS06-055)](http://blogs.technet.com/msrc/archive/2006/09/26/459194.aspx) for the VML vulnerability I mentioned last week. As usual, it's available via Windows Update, WSUS, and various other distribution mechanisms.
