---
author: slowe
categories: Rant
comments: true
date: 2007-11-06T10:03:31Z
slug: microsoft-update-troubles-again
tags:
- Microsoft
- Windows
title: Microsoft Update Troubles Again
url: /2007/11/06/microsoft-update-troubles-again/
wordpress_id: 570
---

It seems like Microsoft just doesn't learn. OK, I'll grant you, it's hard for a multi-billion dollar company with tens of thousands of employees to move quickly or respond nimbly to customer concerns, but I can only justify their actions so far. Here we are again, for the second time in as many months, talking about Microsoft pushing software to Windows machines when that software was unapproved or unwanted.

Last time something like this happened, Microsoft was updating their Windows Auto Update client, even when automatic updates were turned off. The explanation was something along the lines of "we needed to update the auto-update client so it would work", failing to recognize that if it was turned off it didn't really need to work, did it?

This time, we find Microsoft unexpectedly changing the applicability scope for an update delivered via Windows Server Update Services (WSUS) and delivering that update as a revision. Because most WSUS installations are set to auto-approve revisions---this helps reduce the administrative overhead of managing the update list---this update was pushed out to all systems. And because this update was a full installation, not just an update, then suddenly organizations find themselves with loads of machines running Windows Desktop Search (WDS). Oh, that includes the servers in the datacenter, too.

The WSUS team at Microsoft posted a blog entry [trying to explain the behavior](http://blogs.technet.com/wsus/archive/2007/10/25/wds-revision-update-expanded-applicability-rules-auto-approve-revisions.aspx) and, in my opinion, failing miserably. Since this was technically classified as a revision to an existing update, the WSUS team insists that this revision would _only_ have been installed if prior WDS updates had also been approved. This is consistent with the automatic approval of revisions.

Unfortunately, it would appear that Microsoft made poor judgment calls in a number of areas:

* Listing this as a revision (which is supposed to be limited to changes in metadata or applicability rules, never changes in binaries) when in fact it appears to be an entirely new version of WDS

* Changing the applicability rules for this update to include all systems, including those that did not previously have WDS installed

* According to some customers, installing the update even when previous updates were **not** approved

In my opinion, this bodes poorly for Microsoft. Managing patches is already a huge task for many admins. Now administrators have to worry about "updates" getting installed that shouldn't have been because there's been an applicability change to the update, or it was listed as a revision when it's really a new set of binaries.

If by chance you were hit with this, you can uninstall WDS with one of two different commands. You can use this command:

	%windir%\$ntuninstallkb917013\spuninst\spuninst /quiet /norestart

Or this command:

	MsiExec.exe /X{E72019B8-1287-4093-BE9B-1CFA7BA1A8D2} /quiet /norestart

Are there any readers out there who were actually affected by this issue?
