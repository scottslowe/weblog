---
author: slowe
categories: Information
comments: true
date: 2007-07-09T12:55:01Z
slug: uac-and-ktpassexe
tags:
- ActiveDirectory
- Microsoft
- Security
- Windows
title: UAC and ktpass.exe
url: /2007/07/09/uac-and-ktpassexe/
wordpress_id: 486
---

User Account Control (UAC) is a feature new to [Windows Vista](http://www.microsoft.com/windowsvista/) and [Windows Server 2008](http://www.microsoft.com/windowsserver2008/default.mspx) that is designed to help protect Windows-based systems against processes running with administrative permissions. It's a great idea, but the implementation is, in my humble opinion, a bit flawed.

Here's a great example. In working on interoperability and integration documentation for Windows Server 2008, I came across a problem that prevents you from using [Samba](http://www.samba.org/) to join Linux or UNIX systems to Active Directory for the purpose of centralizing authentication to those systems (more information available in [this article][1]). OK, no big deal; we've done it before with ktpass.exe, right? We'll just drop back to using ktpass.exe and do it "old school".

Here's the output from ktpass.exe when running on a Windows Server 2008-based server with UAC enabled:

```text
ktpass.exe -princ HOST/vsxsoltest01.vmwarelab.net@ADNG.VMWARELAB.NET 
-mapuser ADNG\VSXSOLTEST01$ -crypto all -pass Password123 
-ptype KRB5_NT_PRINCIPAL -out c:\vsxsoltest01.keytab  

Targeting domain controller: vswdcng02.adng.vmwarelab.net  

Using legacy password setting method  

Failed to set property "servicePrincipalName" to 
"HOST/vsxsoltest01.vmwarelab.net" on Dn 
"CN=VSXSOLTEST01,CN=Computers,DC=adng,DC=vmwarelab,DC=net": 0x32.  

WARNING: Unable to set SPN mapping data.  

If VSXSOLTEST01$ already has an SPN mapping installed for 
HOST/vsxsoltest01.vmwarelab.net, this is no cause for concern.  

WARNING: Account VSXSOLTEST01$ is not a user account (uacflags=0x1021).  

WARNING: Resetting VSXSOLTEST01$'s password may cause 
 authentication problems if VSXSOLTEST01$ is being used as a server.  

Reset VSXSOLTEST01$'s password [y/n]?  y  

Aborted.
```

This is running as an account that is not the built-in Administrator account, but is a member of Domain Admins, Schema Admins, Enterprise Admins, and the built-in Administrators group.

Take that same command and run it on the same server after disabling UAC, and it runs just fine. No errors, no warnings, no problems. Clearly, UAC is interfering with ktpass.exe.

If you have a need to integrate Linux and/or UNIX systems into Active Directory for authentication, keep this in mind: you'll need to disable UAC (and reboot the server) before you can use ktpass.exe to map service principals onto accounts.

[1]: {{< relref "2007-07-09-samba-and-windows-server-2008-interoperability.md" >}}
