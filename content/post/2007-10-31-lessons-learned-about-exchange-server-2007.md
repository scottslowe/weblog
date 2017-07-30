---
author: slowe
categories: Rant
comments: true
date: 2007-10-31T23:19:32Z
slug: lessons-learned-about-exchange-server-2007
tags:
- ActiveDirectory
- Exchange
- Microsoft
- Windows
title: Lessons Learned About Exchange Server 2007
url: /2007/10/31/lessons-learned-about-exchange-server-2007/
wordpress_id: 567
---

I'm in the midst of a non-virtualization-related project right now, which is a bit odd; a great majority of my work these days is centered around virtualization. Nevertheless, I try to view every project as one from which I can learn. I have definitely learned some things with this project, that's for sure.

Here are a few tidbits that I've learned so far this week, most of them centered around the installation of Microsoft Exchange Server 2007.

## No GUI Setup

First, if you have even _one_ Active Directory domain controller that isn't running Windows Server 2003 SP1, you can't use the GUI setup routine for installing Exchange Server 2007. That's right, no GUI setup for you. Instead, you'll have to install from the command line like this:  

	setup.com /mode:install /roles:mb, ca, ht, mt /EnableLegacyOutlook /LegacyRoutingServer:oldserver.domain.com /dc:win2k3sp1.domain.com

Nice, eh? Supposedly this will be fixed in Exchange 2007 SP1.

## Failures with Local CD-ROMs

Apparently, about 20% of the installations run from the command-line fail with an error about being unable to access the source files. This is even when installing from local CD-ROM, as I was. The Microsoft tech I spoke with recognized that this was a problem; the suggested solution was to copy the files from the CD to a local hard drive and run setup from there.

## LegacyRoutingServer Switch

The use of the "/LegacyRoutingServer:" command-line switch, which is required for interoperability with "legacy" Exchange 2000/2003 servers, can only be used when installing the very first server with the Hub Transport role (the "ht" in the command line above). If the installation of that first server dies for some reason---say, like due to some strange error about not being able to access the source files, even if they are on a local CD-ROM---then you won't be able to use this command-line switch again. This means you'll need to create the appropriate connector yourself manually after installation.

## CAS Installation Failure

If Exchange Server 2007 setup fails while installing the Client Access server role (the "ca" in the setup command line above) citing an error about not being able to find an object (see [this URL](http://technet.microsoft.com/en-us/library/bb738162.aspx)), then you've got some damaged attributes in Active Directory. In my case, while sitting on hold with Microsoft Support for an hour, I resolved it by doing a full dump of the domain and configuration naming contexts of Active Directory using `ldifde`:

	ldifde -f example.domain.ldf -d "dc=example,dc=com"  
	ldifde -f example.config.ldf -d "cn=configuration,dc=example,dc=com"

I was then able to find the specific object with which Exchange Server 2007 Setup was having a problem and fix it. In my case, the Offline Address Book server had somehow gotten damaged and was causing setup to fail. I was able to manually correct the problem using Exchange 2000 System Manager and then Exchange Server 2007 setup proceeded to completion.

## SMTP Routing Loop with Smart Host

Specifying a smart host on the SMTP virtual server properties on your "legacy" Exchange servers will cause a routing loop, and mail won't flow between the new and old servers. Apparently this is documented somewhere, although the Microsoft tech I spoke to could only point me to some articles about how to configure a smart host. I haven't seen any documentation yet that recommends checking and fixing this potential problem. Furthermore, the troubleshooting tools in Exchange Server 2007 pick this up, but fail to tell you that it could be a problem.

## ASP.NET Required

Oh, yes, I almost forgot about one: ASP.NET is required for Exchange Server 2007, but what happens when you can't install it via Add/Remove Programs > Add/Remove Windows Components? That's right, back to the command line again:  

	%systemroot%\Microsoft.NET\Framework64\v2.0.50727\aspnet_regiis.exe -ir -enable

This is assuming, of course, that you've already installed the .NET Framework 2.0 on your server in preparation for Exchange.

You are welcome to tell me that I'm an idiot for not knowing this stuff, on one condition: you provide a URL where information about the problem is posted and a workaround provided. That way, when someone else runs into the issue, we'll at least know where to point them for help.
