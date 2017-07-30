---
author: slowe
categories: News
comments: false
date: 2006-01-10T09:07:41Z
slug: more-wmf-flaws-uncovered
tags:
- Microsoft
- Security
- Vulnerability
- Windows
title: More WMF Flaws Uncovered
url: /2006/01/10/more-wmf-flaws-uncovered/
wordpress_id: 153
---

And here I thought things would settle down a bit now that Microsoft has released [MS06-001](http://www.microsoft.com/technet/security/bulletin/MS06-001.mspx), the patch for the previous exploitable WMF flaw. Now, just days after the release of that out-of-band security patch, more WMF flaws have been uncovered.

This article describes some [new WMF flaws](http://www.eweek.com/article2/0,1759,1909445,00.asp) recently uncovered and announced on the BugTraq mailing list. (See [this article](http://www.informationweek.com/news/showArticle.jhtml?articleID=175802831) for more information as well.) Apparently, [Symantec](http://www.symantec.com/) issued a warning via its Deepsight Management System, but I was unable to find an available online copy of that advisory.

Perhaps the most disturbing thing to me is the fact that Microsoft already _knew_ about these newly disclosed problems. In a [Microsoft Security Response Center (MSRC) blog posting](http://blogs.technet.com/msrc/archive/2006/01/09/417198.aspx), Lennart Wistrand (a lead security program manager for the MSRC) indicated that these problems had already been identified by Microsoft during code review, but were not going to be addressed as part of the MS06-001 security patch because "These issues do not allow an attacker to run code or crash the operating system. They may cause the WMF application to crash, in which case the user may restart the application and resume activity."

OK, so we know that we have a flaw in our software. We know that other flaws in the same portion of software have already proven to be remotely exploitable and can result in remote code execution. (Keep in mind that Microsoft also released a WMF patch last year as [MS05-053](http://www.microsoft.com/technet/security/bulletin/MS05-053.mspx).) It is certainly possible that these flaws could result in remote code execution, although at this time all that we've seen are denial of service and application crashes. Yet, we decide that we are not going to fix the flaw until the next Service Pack, whenever that might be? That's just crazy, in my opinion.

Fortunately for end users, remote code execution is not a possibility (yet) with this flaw, and it does not appear to be as exploitable as the flaws patched with MS06-001. If you haven't already, go ahead and patch systems with the MS06-001 fix, and then stay tuned for further developments with these newly disclosed vulnerabilities. That's about the best anyone can do under the circumstances.
