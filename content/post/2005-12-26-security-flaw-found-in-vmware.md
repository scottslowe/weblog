---
author: slowe
categories: News
comments: false
date: 2005-12-26T21:45:32Z
slug: security-flaw-found-in-vmware
tags:
- Security
- Virtualization
- VMware
title: Security Flaw Found in VMware
url: /2005/12/26/security-flaw-found-in-vmware/
wordpress_id: 146
---

Just so the Windows flaws don't get all the attention, here's a reasonably new one: a flaw in [VMware](http://www.vmware.com/), the machine virtualization software that is tremendously popular, especially among security professionals. Fortunately, it's reasonably easy to work around the flaw, and VMware has already released a patch.

This link offers more details on the [VMware security flaw](http://www.eweek.com/article2/0,1759,1904647,00.asp), which apparently affects almost all of the VMware products, with the notable exception of ESX Server. VMware's [security advisory](http://www.vmware.com/support/kb/enduser/std_adp.php?p_faqid=2000) also provides complete details and information on how to protect against it, plus a link to download the patch.

VMware users can sidestep the security issue by not using NAT networking for their virtual machines, as pointed out in the security advisory.

While security flaws in Microsoft's Windows operating system and Internet Explorer web browser get most of the attention these days, all products have them. Users of Linux or [Mac OS X](http://www.apple.com/macosx/) like to proclaim their immunity, but no one is immune. This recent vulnerability merely highlights the fact that we must all be vigilant and watchful in order to maintain the security and integrity of our networks and systems.
