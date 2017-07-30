---
author: slowe
categories: Information
comments: false
date: 2005-10-08T17:58:09Z
slug: just-in-case
tags:
- ActiveDirectory
- Microsoft
- Security
title: Just in Case
url: /2005/10/08/just-in-case/
wordpress_id: 102
---

I've mentioned the bug discussed in [KB905809](http://support.microsoft.com/default.aspx?scid=kb;en-us;905809) several times in this blog. In reading those posts (or reading the KB article), you've probably seen that you can use the `SC.EXE` command to set the security descriptors on the Service Control Manager to fix the bug. What happens, though, if you mess up the `SC.EXE` command?

It turns out that this very thing happened recently. A systems administrator copied the required `SC.EXE` command line from a Word document, thinking it was safer to copy and paste than try to type the command line directly. As it turns out, the copied text from Word had a carriage return in it, and this caused the command line to terminate early (before all the security descriptors were applied). In fact, what happened is that all of the "Write" permissions were removed.

If you run `sc sdshow scmanager` on a system running Windows Server 2003 SP1, you'll get this:

`D:(A;;CC;;;AU)(A;;CCLCRPRC;;;IU)(A;;CCLCRPRC;;;SU)`  
`(A;;CCLCRPWPRC;;;SY)(A;;KA;;;BA)S:(AU;FA;KA;;;WD)`  
`(AU;OIIOFA;GA;;;WD)`

(Side note: The first section is the problem area, as this is what grants permission to the Authenticated Users group.  In the fix described in KB905809, this section changes to grant Authenticated Users more permissions.)

In this particular situation, all of the "Write" permissions---everything after the `(A;;CCLCRPRC;;;SU)` section---was accidentally removed. So, when the administrator realized what had happened and went back to try to fix it the system kicked back an "Access Denied" error message, because the permissions had been removed. With no permission to change the permissions, what could be done?

Fortunately, we found a fix. This [URL](http://www.exchangeserveradmin.org/ftopic24842_Could_NOT_change_mail_address_after_windows_server_2003_sp1.html) describes a Registry key that someone else had concocted trying to fix this very problem. In my discussions with Microsoft Product Support services, they analyzed this Registry hack and determined that the end result was the same as the `SC.EXE` command. By importing this Registry fix, we were able to restore the permissions that had been inadvertently removed. We then used SC again (with `sdshow scmanager`) to verify that the permissions were as they needed to be.

We are fortunate that this Registry fix solved the problem. Had the Registry fix not worked, we most likely would have had to rebuild the server in question---not a pleasant thought given that we had several hundred users already migrated over to that server.

What can we learn from this situation? Here are my thoughts:

1. Always type your commands by hand, instead of copying and pasting.

2. Take appropriate precautions. In large-scale production environments, pull a hot-plug hard drive from the RAID array first. If things go south, shut the server down and reboot from the pristine drive.

3. When running a command like this, pray.

The real question, in my mind, is this: why isn't Microsoft going to fix this bug? We may never know the answer to that question.
