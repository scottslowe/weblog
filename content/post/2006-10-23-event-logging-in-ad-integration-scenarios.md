---
author: slowe
categories: Information
comments: true
date: 2006-10-23T09:54:02Z
slug: event-logging-in-ad-integration-scenarios
tags:
- ActiveDirectory
- CentOS
- Interoperability
- Kerberos
- LDAP
- Linux
- Microsoft
- Security
- Solaris
- UNIX
- Windows
title: Event Logging in AD Integration Scenarios
url: /2006/10/23/event-logging-in-ad-integration-scenarios/
wordpress_id: 348
---

To test what kind of logging occurs, I simply used my existing installation of [Windows Server 2003 R2](http://www.microsoft.com/windowsserver2003/default.mspx) and Active Directory, for which the logging options had not been modified from their "out of the box" settings. From there, I cleared the security event logs and then attempted an SSH login to a [CentOS](http://www.centos.org/) 4.3 server that has been configured for AD authentication via Kerberos (through PAM, not directly inside SSH).

After the login, I reviewed the event logs and found a large number of entries for the LDAP bind account (the account that is used to bind to Active Directory to retrieve account information, such as UID number, GID number, login shell, home directory, etc.). These are useless for identifying unique logons to Linux/Unix-based systems. However, there is one event that is useful---an event ID 672 with the following text:

```text
    Authentication Ticket Request:
     	User Name:		bjones
     	Supplied Realm Name:	EXAMPLE.NET
     	User ID:			EXAMPLE\bjones
     	Service Name:		krbtgt
     	Service ID:		EXAMPLE\krbtgt
     	Ticket Options:		0x40800000
     	Result Code:		-
     	Ticket Encryption Type:	0x17
     	Pre-Authentication Type:	2
     	Client Address:		172.16.28.111
     	Certificate Issuer Name:	
     	Certificate Serial Number:	
     	Certificate Thumbprint:	
    
    For more information, see Help and Support Center at 
    http://go.microsoft.com/fwlink/events.asp.
```

As you can see, this entry clearly identifies the user that was authenticated as well as the IP address of the host to which the user logged in. Note that the IP address of the workstation from which the user logon originated is not captured here.

OK, that takes care of the typical Linux system. Now, what about [Solaris](http://www.sun.com/software/solaris/) 10? I cleared the security event logs (saving them) and attempted an SSH logon to a Solaris server. As it turns out, Solaris 10 logs two event entries---event ID 672, as above, as well as event ID 673. The text for event ID 673 is as follows:

```text
    Service Ticket Request:
     	User Name:		bjones@EXAMPLE.NET
     	User Domain:		EXAMPLE.NET
     	Service Name:		host-solarishost01
     	Service ID:		EXAMPLE\host-solarishost01
     	Ticket Options:		0x40800000
     	Ticket Encryption Type:	0x3
     	Client Address:		172.16.28.112
     	Failure Code:		-
     	Logon GUID:		{3a653f45-928c-1f72-2c59-ca951986ac42}
     	Transited Services:	-
    
    For more information, see Help and Support Center at 
    http://go.microsoft.com/fwlink/events.asp.
```

This provides the same sort of information as well as the name of the service ticket that was requested by the host. (Keep in mind, too, that Solaris 10 logged both the event ID 672 _and_ event ID 673, whereas CentOS logged only the event ID 672.)

But what about "native" Kerberos authentication, when a Kerberos ticket is used to authenticate the user? In this case, I tested three different operating systems: CentOS 4.3, Solaris 10, and [OpenBSD](http://www.openbsd.org/) 3.9. In all three cases an event ID 673, similar to above, was logged. However, this time it was the IP address of my actual workstation---not the IP address of the server---that was included in the event log text.

In all of the tested scenarios, there was information that identified the specific account that was being authenticated. However, depending upon whether PAM was involved, the Windows event logs may or may not capture the actual IP address of the originating workstation. In almost all cases, though, the logs on the Linux, Solaris, or OpenBSD server would capture the IP address of the originating workstation, even if the Windows event logs did not.
