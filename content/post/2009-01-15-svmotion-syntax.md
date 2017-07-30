---
author: slowe
categories: Information
comments: true
date: 2009-01-15T16:08:25Z
slug: svmotion-syntax
tags:
- CLI
- Storage
- Virtualization
- VMware
title: SVMotion Syntax
url: /2009/01/15/svmotion-syntax/
wordpress_id: 1137
---

This is nothing earth-shattering; I just needed to record the syntax for the SVMotion command in the Remote CLI so that next time I need it six months from now, I'll have an easy reference.

Here's the syntax:

	svmotion --datacenter=<DC Name> --url=https://<FQDN of VCMS>/sdk
	--username=<Username on VCMS> --password=<Password> --vm=[<Source
	datastore>] <Path to VMX file of VM>:<Destination datastore>

If any parameters have spaces in them---like the datacenter name or the path to the .VMX file---you'll need to enclose those parameters in single quotes.
