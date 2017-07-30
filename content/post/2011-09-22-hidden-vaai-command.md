---
author: slowe
categories: Explanation
comments: true
date: 2011-09-22T13:32:18Z
slug: hidden-vaai-command
tags:
- Storage
- Virtualization
- VMware
- vSphere
title: Hidden VAAI Command
url: /2011/09/22/hidden-vaai-command/
wordpress_id: 2429
---

As some of you are probably already aware, one of the storage-related features added to vSphere 5 is support for the SCSI UNMAP command. While you would normally want this functionality enabled, there could be instances where you might want to disable this functionality. Unfortunately, there's no option in the user interface to enable or disable SCSI UNMAP support.

However, you _can_ use `esxcli` to enable or disable UNMAP support:

	esxcli system advcfg setvalue --int-value [0|1] --option /VMFS3/EnableBlockDelete

Setting this value to 0 disables SCSI UNMAP support; setting the value to 1 enables it.

Many thanks to Cormac Hogan of VMware and Cody Hosterman of EMC for their help with this command.

**Update:** I received word back from Cormac that the syntax above was lifted from a beta build; the correct syntax (thanks Nick T!) is as follows:

	esxcli system settings advanced set --int-value [0|1] --option /VMFS3/EnableBlockDelete

You can use this command to list the current status:

	esxcli system settings advanced list --option /VMFS3/EnableBlockDelete

Thanks for all the comments and additional information!
