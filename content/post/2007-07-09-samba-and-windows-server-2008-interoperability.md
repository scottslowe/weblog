---
author: slowe
categories: Information
comments: true
date: 2007-07-09T11:13:32Z
slug: samba-and-windows-server-2008-interoperability
tags:
- Interoperability
- Kerberos
- Samba
- Windows
title: Samba and Windows Server 2008 Interoperability
url: /2007/07/09/samba-and-windows-server-2008-interoperability/
wordpress_id: 485
---

[Samba](http://www.samba.org/), as I'm sure you already know, is an open source implementation of SMB/CIFS for UNIX, Linux, and similar operating systems. I've found Samba to be extremely helpful in providing some assistance for integration into Active Directory, as evidenced by these articles:

[Solaris 10-AD Integration, Version 3][1]  
[Linux-AD Integration, Version 4][2]

Both of these articles utilize Samba's Active Directory support to help automate the process of joining non-Windows systems to Active Directory for the purpose of authenticating logon requests against Active Directory.

So when it came time to start working on integrating Linux or Solaris into Active Directory on [Windows Server 2008](http://www.microsoft.com/windowsserver2008/default.mspx), I naturally assumed that I'd be able to use Samba in the same way as I had before. Unfortunately, that's not the case. Due to changes in Windows, and due to the fact that previous versions of Windows were non-standard (i.e., didn't follow the RFCs---surprise, surprise), using Samba to join an Active Directory domain running on Windows Server 2008 currently does not work.

This [thread on the Samba mailing list](http://www.nabble.com/SPNEGO-in-Samba---Longhorn-Server-interop-issues...-t4021562.html) provides some additional information, and this [Google search](http://www.google.com/search?hl=en&q=not_defined_in_RFC4178&btnG=Search) turns up a few hits that also provide additional information on the problem. Until this particular issue is resolved, we won't be able to use the `net ads join` commands (and potentially some others as well) against Active Directory domains running on Windows Server 2008. Looks like it's back to `ktpass.exe` again!

[1]: {{< relref "2007-04-25-solaris-10-ad-integration-version-3.md" >}}
[2]: {{< relref "2007-01-15-linux-ad-integration-version-4.md" >}}
