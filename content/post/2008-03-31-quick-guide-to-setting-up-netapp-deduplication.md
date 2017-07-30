---
author: slowe
categories: Education
comments: true
date: 2008-03-31T16:31:47Z
slug: quick-guide-to-setting-up-netapp-deduplication
tags:
- Hardware
- NAS
- NetApp
- NFS
- ONTAP
- Storage
title: Quick Guide to Setting up NetApp Deduplication
url: /2008/03/31/quick-guide-to-setting-up-netapp-deduplication/
wordpress_id: 671
---

I'm relatively new to NetApp deduplication (formerly A-SIS), so this article won't be an advanced treatise on NetApp deduplication or its deep inner workings. Instead, this is intended to be a quick guide to setting up NetApp deduplication for others, like myself, who may be familiar with Data ONTAP but not necessarily deduplication.

Obviously, the first step will be to ensure that your NetApp storage system is licensed for deduplication. As of March 10, NetApp made the NearStore option, which was a prerequisite for deduplication, free. Yes, you read that right: free. Since NearStore is a prerequisite, you'll need to be sure to license that first:

	license add <Code for NearStore>  
	license add <Code for Deduplication>`

Once deduplication is licensed, then you can enable it on a _per-volume_ basis using the `sis on` command:

	sis on /vol/<volname>

Note, however, that the volume cannot exceed a certain size, based on the storage system model, in order for deduplication to work. These volume size limits are laid out in [TR3505](http://media.netapp.com/documents/tr-3505.pdf). Note that the volume must never have been any bigger than the size limits described, so this means you can't size it down to the limits set forth and then run deduplication.

Once it's running, you can check the status with:

	sis status /vol/<volname>

After it's finished running, you can see your space savings like this:

	df -s /vol/<volname>

After running deduplication on a small NFS volume that housed only three VMs, the `df -s` command showed a space savings of 64%. That's pretty impressive!

Moving forward, deduplication will run automatically every night at midnight, as shown by this command:

	sis config /vol/<volname>

That should be enough to get most everyone started. Feel free to post comments or corrections below.

**UPDATE:** I've corrected the broken TR-3505 link. Thanks, Tom!
