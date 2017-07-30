---
author: slowe
categories: Education
comments: true
date: 2011-08-12T09:00:00Z
slug: a-deeper-look-at-vasa
tags:
- Storage
- Virtualization
- VMware
- vSphere
title: A Deeper Look at VASA
url: /2011/08/12/a-deeper-look-at-vasa/
wordpress_id: 2368
---

Much has been said and written about VASA (the vSphere APIs for Storage Awareness), a key part of vSphere 5 and the "magic" behind the new profile-driven storage functionality. I recently had the opportunity to dive a bit deeper into VASA, and discovered some information that I felt was important to share with the virtualization community. This post probably won't earn me any brownie points at VMware, but at least we'll all have a better idea of what VASA can and cannot do.

The basic premise of VASA is that something called the VASA provider (also referred to as a storage provider in the vSphere Client UI) supplies something called _capabilities_ about datastores. It is widely believed that for each datastore the VASA provider will pass up multiple capabilities like deduplication, replication, snapshot status, RAID level, drive type, or performance (IOps/MBps) capacities. vSphere administrators could then use some arbitrary combination of these capabilities to create a VM storage profile. VM storage profiles can then help determine which datastores are compatible (support the necessary capabilities) or incompatible (don't support the required capabilities) when creating new VMDKs, performing Storage vMotion operations, cloning a VM, or deploying a VM from a template.

This sounds pretty cool, right? Unfortunately, **_that's not how VASA works._**

The VASA protocol actually limits the storage provider to supplying (or assigning) only one capability to each datastore. That's right---each datastore can have only one capability provided by the VASA provider (a capability assigned by the storage provider is also referred to as a system-provided capability). So instead of VASA providing up individual attributes like RAID level, drive type, etc., and allowing vSphere administrators to build VM storage profiles from these attributes, the VASA provider must supply a single "meta-attribute" that is a sum of the various configuration options. This means you'll see system-provided capabilities like:

* A concatenation of various attributes lumped together in a single label, like "RAID 5;Thin;Deduplicated;SAS/FC Drives"

* A generic but descriptive label, like "High Performance"

* A fully generic label such as "Gold" or "Silver"

&lt;aside&gt;At this point it should be fairly obvious that the use of the word "capability" in the manner that VASA and profile-driven storage use it is horribly misleading. "Label," as I've used above, is far more accurate.&lt;/aside&gt;

The VASA provider will then assign **one** of these system-provided capabilities to the datastore. vSphere administrators have the option of applying **one** user-defined capability to a datastore as well. This means that, at most, any given datastore may have a maximum of two capabilities: one system-provided and one user-defined.

Because your datastores may have no more than two capabilities assigned, your VM storage profiles can also have no more than two capabilities selected. Think about it---if you select three capabilities (which the UI will let you do), all datastores will be found incompatible because none of them have all three capabilities.

This behavior is a far cry from what many people expected and believed VASA would do, myself included. I expected VASA to be far more powerful and far more flexible than it is, and I would imagine that a fairly significant number of readers were under the same impression.

I'd love to hear from the readers. What were your impressions of what VASA would deliver, and how do they compare to the reality? Feel free to share your thoughts in the comments below.
