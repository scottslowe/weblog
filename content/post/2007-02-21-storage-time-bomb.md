---
author: slowe
categories: Rant
comments: true
date: 2007-02-21T22:42:41Z
slug: storage-time-bomb
tags:
- SAN
- Security
- Storage
- Virtualization
- VMware
title: Storage Time Bomb?
url: /2007/02/21/storage-time-bomb/
wordpress_id: 417
---

The "ticking storage time bomb," as described in [this article](http://www.techworld.com/opsys/features/index.cfm?featureID=2257), is the fact that all virtual servers share the same WWN (World Wide Name) on the Fibre Channel HBA.

Quoting from the article:

>In a virtual server mode, all of the server instances can see and access the same HBA - and all the same logical unit numbers (LUN) attached to it.

Fair enough, at first glance. At first glance, you might say, "Wow! He's right---all those servers share the same HBA going out to my Fibre Channel SAN, and I'm doing all my zoning and LUN presentation based on HBA. I'm in trouble!"

But look a little deeper and it appears that there is a fundamental flaw in this argument: _The virtual servers don't have access to the HBA._ In fact, they don't even know the HBA exists. They don't know the SAN exists. Even if they knew the HBA and the SAN existed, they still don't have direct access to that hardware---they have to go through the virtualization layer.

What driver does a Windows guest running on VMware use to access its hard disk? An LSI Logic or BusLogic SCSI controller.  Not an Emulex HBA driver, or a Qlogic HBA driver, but a standard, ordinary SCSI controller. So how exactly will this Windows guest gain access to LUNs on a SAN that it doesn't know exists and for which it has no drivers? Or am I just missing something here?

Unless I'm way off (which is certainly very possible), this article is _completely misguided._ Yes, N-Port Virtualization is a good thing, but not necessarily for security. Yes, zoning and LUN masking are important components of a Fibre Channel SAN, but those concepts don't apply to virtual servers who don't know the SAN exists, don't have any drivers for SAN hardware, don't have any SAN hardware, and couldn't access the SAN directly if they wanted to. You have to remember that VMware (and presumably Xen and others, although I don't know that for certain) are hiding these details from the guest operating systems.

Am I missing some crucial detail?

**UPDATE:** Some of you may have already considered this fact, but add this to the equation: [VMware Consolidated Backup](http://www.vmware.com/products/vi/consolidated_backup.html) uses a backup proxy server, running Windows Server 2003 or later, that must have access to the same SAN LUNs as the ESX Server hosts. In this instance, I _would_ consider this to be a potential security problem. Make sure you properly secure and harden the backup proxy!
