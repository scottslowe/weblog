---
author: slowe
categories: Information
comments: false
date: 2005-09-20T17:01:38Z
slug: inability-to-add-or-modify-e-mail-addresses
tags:
- Microsoft
- ActiveDirectory
- Windows
title: Inability to Add or Modify E-Mail Addresses
url: /2005/09/20/inability-to-add-or-modify-e-mail-addresses/
wordpress_id: 88
---

This is a follow-up to my posting titled "[Another Funky AD Issue][1]", in which I described a situation with a customer where support staff are unable to modify or add an e-mail address to a mail-enabled or mailbox-enabled user or contact after it has been created.

It turns out that this is a bug in Windows Server 2003 SP1. The [KB article][2] I referenced in the earlier blog posting is also slightly inaccurate; what that article portrays as two separate workarounds to the problem are actually two steps in one workaround. So, in reality, you'll need to perform **both** steps (both the `SC.EXE` command and the Group Policy Object) in order to fix the problem. (Please note that I have not verified this myself yet.)

As it currently stands, the KB article is the only known workaround for this problem. According to the Microsoft Product Support Services people I've spoken with thus far, it is unknown if Microsoft will issue a hotfix to correct the problem. In the meantime, I have forwarded a URL with another potential fix (see [this URL][3]) for Microsoft to analyze and determine if this will help the situation.

Other potential solutions include adding the support staff to the local Administrators group on the Exchange server(s); this, clearly, is a less-than-ideal solution. Supposedly, removing Windows Server 2003 SP1 will also correct the problem. (Again, I have not verified this personally.)

Microsoft Product Support Services is supposed to get back in touch with me in the next couple of days with some additional information. I'll post more information here as it becomes available.

[1]: {{< relref "2005-09-14-another-funky-ad-issue.md" >}}
[2]: http://support.microsoft.com/default.aspx?scid=kb;en-us;905809
[3]: http://www.exchangeserveradmin.org/ftopic24842_Could_NOT_change_mail_address_after_windows_server_2003_sp1.html