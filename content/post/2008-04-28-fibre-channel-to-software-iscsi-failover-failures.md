---
author: slowe
categories: Rant
comments: true
date: 2008-04-28T14:00:29Z
slug: fibre-channel-to-software-iscsi-failover-failures
tags:
- ESX
- iSCSI
- NetApp
- Storage
- Virtualization
- VMware
title: Fibre Channel to Software iSCSI Failover Failures
url: /2008/04/28/fibre-channel-to-software-iscsi-failover-failures/
wordpress_id: 695
---

What I had hoped to be able to publish today would be an article describing how to configure and use ESX's software iSCSI initiator as a failover path for Fibre Channel, so that if the Fibre Channel fabric completely failed VM traffic would automatically failover to software iSCSI. I thought that this would be a great, low-cost way to add another layer of redundancy to your VMware ESX environment.

Unfortunately, I can't make it work. Here's the setup I've been using for testing:

* A 200GB LUN visible to ESX over both Fibre Channel (FC) and software iSCSI

* A VM, stored on this LUN, running Windows Server 2003 R2

Initial tests led me to believe that it would indeed work. I verified that both the FC path as well as the iSCSI path were listed as separate paths for the same LUN. Without placing any load on the VM, I pulled the FC connection from the back of the server. The VM stayed up, and I was able to browse the local hard drive inside the VM. Network connectivity remained active. And the "Manage Paths" dialog box even showed the FC connection as "Dead" and the iSCSI connection as On/Active. Given that information, it seemed like all was good.

Determined to verify that it was working as I expected, I trotted out a copy of IOmeter and tried to repeat the tests. This time around, though, the tests did not go quite so well. IOmeter showed that disk throughput stopped, and the VI Client locked up. I repeated this set of tests a couple of times, and each time---while IOmeter was running---I ran into issues.

Based on these results, I'm inclined to say that one of two things is true. Either:

1. I did something very, very wrong; or

2. ESX isn't quite right to support automatic failover between FC and software iSCSI.

Has anyone else tried this, or am I the only one? If you have tried it, did it work? If so, what steps did you have to take---if any---to make it work properly?
