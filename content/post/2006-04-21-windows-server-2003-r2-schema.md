---
author: slowe
categories: Information
comments: false
date: 2006-04-21T22:05:18Z
slug: windows-server-2003-r2-schema
tags:
- ActiveDirectory
- Microsoft
- Windows
title: Windows Server 2003 R2 Schema
url: /2006/04/21/windows-server-2003-r2-schema/
wordpress_id: 231
---

I came across this little tidbit on the Microsoft public newsgroups for [Windows Server 2003 R2](http://www.microsoft.com/windowsserver2003/). It concerns adding a Windows Server 2003 R2 domain controller to an existing Windows 2000-based Active Directory domain.

Most of you are probably aware that before you can add a Windows Server 2003-based domain controller to a Windows 2000-based Active Directory domain, you must extend the Active Directory schema using ADPrep. So far, so good.

Some of you may also be aware that Windows Server 2003 R2 is really nothing more than Windows Server 2003 with SP1 and some additional components. In fact, if you don't run the setup on CD 2 of the 2-disc set for Windows Server 2003 R2, you end up with Windows Server 2003 with SP1. You have to install the second disc in order to get R2. OK, still with me?

Here's the kicker. In order to add a Windows Server 2003 R2 to a Windows 2000-based Active Directory domain, you must run ADPrep from _the second disc_, not the first disc. There is still an `ADPrep.exe` on the first disc, but it doesn't extend the schema far enough to support R2. So, unsuspecting admins install R2, run ADPrep from the first CD, then try to run DCPromo and get an error regarding the schema. However, running ADPrep from the second disc _will_ extend the schema properly and allow DCPromo to complete.
