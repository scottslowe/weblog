---
author: slowe
categories: Rant
comments: false
date: 2005-09-14T23:08:57Z
slug: another-funky-ad-issue
tags:
- Microsoft
- ActiveDirectory
title: Another Funky AD Issue
url: /2005/09/14/another-funky-ad-issue/
wordpress_id: 84
---

This one is still unresolved. The basic gist of the arrangement is this: user accounts that have been delegated the appropriate permissions in Exchange System Manager and Active Directory in order to be able to manage user objects (including e-mail attributes) are unable to add or edit e-mail addresses on mail-enabled and mailbox-enabled objects. [Microsoft](http://www.microsoft.com/) has a [KB article](http://support.microsoft.com/default.aspx?scid=kb;en-us;905809) that describes the issue perfectly, but the fix doesn't work (at least, not for this specific implementation). The KB article and numerous hits from a [Google](http://www.google.com/) search indicate that the use of `SC.EXE` from Windows Server 2003 SP1 can fix the problem, but it doesn't work, and the other workaround offered by the KB article isn't particularly appealing (using Group Policy Objects in Active Directory to add permissions to a service across the network).
