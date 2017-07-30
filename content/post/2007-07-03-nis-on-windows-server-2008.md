---
author: slowe
categories: Rant
comments: true
date: 2007-07-03T14:22:35Z
slug: nis-on-windows-server-2008
tags:
- Microsoft
title: NIS on Windows Server 2008
url: /2007/07/03/nis-on-windows-server-2008/
wordpress_id: 484
---

Even though NIS is installed as part of the "Identity Management for UNIX" role service (part of the Active Directory Domain Services role) in Windows Server 2008, it appears that some additional steps are required in order to make it work as expected. If anyone has any additional information they'd like to share on this particular issue, please speak up in the comments.

After installing NIS through Server Manager (first installing the Active Directory Domain Services role, then installing Active Directory, then adding the Identity Management for UNIX role service in Server Manager), an error popped up when trying to launch the MMC for Identity Management for UNIX. The error basically stated that no configuration was found and that I needed to run `niscnfg.exe`.

The first time I ran this program (found in `%Systemroot%\IDMU\Setup`), it did nothing. Upon launching the Identity Management for UNIX MMC, I received the same error. Trying to start the Server for NIS service resulted in an error message informing me that the service was currently set to Disabled and therefore could not be started. So I set the service to Manual, started it (it started successfully and without reporting any error messages), and ran `niscnfg.exe` again. It still didn't do anything. I restarted the Identity Management for UNIX MMC and now it seemed to work fine.

It's great that it now works, but the whole situation just creates more questions:

* So, did `niscnfg.exe` actually do anything?

* Was the error message misleading, and really what I needed to do was start the NIS server service?

* Why was the Server for NIS service set to Disabled, anyway, even after I'd installed the Identity Management for UNIX role service?

I'm willing to accept that this issue may be due to the fact that the software is still in beta stage. Any experienced Windows Server 2008 beta testers care to weigh in on this matter?
