---
author: slowe
categories: Information
comments: false
date: 2006-05-04T20:20:57Z
slug: windows-server-2003-r2-as-member-server
tags:
- ActiveDirectory
- Microsoft
- Windows
title: Windows Server 2003 R2 as Member Server
url: /2006/05/04/windows-server-2003-r2-as-member-server/
wordpress_id: 242
---

Another interesting thread on the microsoft.public.windows.server.general newsgroup has turned up an issue with running [Windows Server 2003 R2](http://www.microsoft.com/windowsserver2003/) as a member server.

First of all, many thanks to Jabez Gan, a Windows Server 2003 MVP, for his assistance in clearly defining the scope of this situation. Jabez Gan's weblog is found at [http://www.msblog.org/](http://www.msblog.org/).

It would seem that if you are going to deploy Windows Server 2003 R2 as a member server in a domain that is _not_ running Windows Server 2003 R2 as domain controllers, there are still times when the Active Directory schema must be upgraded. This is a bit unusual, since the addition of newer versions of Windows Server to a domain has typically not required this (think of adding Windows 2000 to a Windows NT domain, or adding Windows Server 2003---not R2---to a Windows 2000-based Active Directory domain).

So, the Active Directory schema will have to be extended if you are planning on deploying any of the following services on a Windows Server 2003 R2-based member server _and_ you are not running Windows Server 2003 R2 on the DCs:

* DFS Replication
* Print Management Console
* File Server Resource Management

Jabez also mentioned UNIX Identity Management, but it seems like that can only be deployed on domain controllers anyway (that's definitely true for Server for NIS). However, in the event that UNIX Identity Management can be deployed to member servers, that will require a schema extension as well.

In summary, if you are planning on deploying some of the newer features of Windows Server 2003 R2 in a domain that is not running Windows Server 2003 R2 on the domain controllers, you may have to extend the Active Directory schema anyway. Be sure to plan and prepare accordingly.
