---
author: slowe
categories: Explanation
comments: true
date: 2009-01-05T17:44:29Z
slug: partial-case-sensitivity-with-esxcfg-advcfg
tags:
- CLI
- ESX
- NFS
- Virtualization
- VMware
title: Partial Case Sensitivity with esxcfg-advcfg
url: /2009/01/05/partial-case-sensitivity-with-esxcfg-advcfg/
wordpress_id: 1120
---

I was working on some documentation this afternoon that centered around the use of `esxcfg-advcfg` to set some NFS-related parameters in VMware ESX. While verifying the command syntax, I noticed something interesting about the `esxcfg-advcfg` command: _it's only partially case-sensitive._

OK, this isn't world-shattering news or a discovery of paramount importance, but I do find this a bit odd given that with the Linux-based Service Console most everything should be entirely case-sensitive. However, I found that with `esxcfg-advcfg` only part of the command needs to be properly capitalized.

Consider these examples. If I wanted to increase the maximum number of NFS datastores on VMware ESX, I would use this command:

	esxcfg-advcfg -s 32 /NFS/MaxVolumes

This command will also work (note the capitalization, or lack thereof):

	esxcfg-advcfg -s 32 /NFS/maxvolumes

But this command won't work; it will report that it can't find the parameter:

	esxcfg-advcfg -s 32 /nfs/maxvolumes

I noticed the same behavior with a couple of other settings, so I'm reasonably confident that it's not just this one setting. It appears that `esxcfg-advcfg` only cares if the first portion of the path is properly capitalized, and the rest of the parameter is case insensitive.

Interesting, no?
