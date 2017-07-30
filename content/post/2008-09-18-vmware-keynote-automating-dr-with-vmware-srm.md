---
author: slowe
categories: Liveblog
comments: true
date: 2008-09-18T12:07:34Z
slug: vmware-keynote-automating-dr-with-vmware-srm
tags:
- Virtualization
- VMotion
- VMware
- VMwareFT
- VMwareHA
- VMwareSRM
- VMworld2008
title: VMware Keynote - Automating DR with VMware SRM
url: /2008/09/18/vmware-keynote-automating-dr-with-vmware-srm/
wordpress_id: 938
---

There is no general session this morning at VMworld 2008; instead, a "keynote" will be delivered about automating disaster recovery (DR) using VMware Site Recovery Manager (SRM). This is similar to the way in which other vendors have delivered various "keynotes" throughout the conference instead of all the announcements being crammed into the morning general sessions.

The speaker this morning is Jay Judkowitz, the product manager for VMware SRM. I've met Jay before; he's a good guy. There's a small technical glitch as the session begins because the slide deck doesn't come up, but that gets resolved within only a few minutes and Jay begins his presentation.

The presentation begins with yet another overview of the VDC-OS vision; SRM is considered one of the vCenter management vServices. Jay then goes on to address all the various ways in which VMware provides application availability for applications hosted on VMware Infrastructure. This would be technologies like VMotion, VMware HA, VMware DRS, VMware FT, NIC teaming, storage multipathing, and of course Site Recovery Manager.

The traditional challenges of DR (including complex recovery processes and procedures, hardware dependence, inability to test extensively or repeatedly) are all addressed by VMware SRM. More accurately, they are addressed by the products that form a foundation underneath VMware SRM. Features like hardware independence, encapsulation, partitioning and consolidation, and resource pooling. These features have a direct play in a DR environment. It's funny to see Jay taking this particular approach; it's almost like he's using the same slide deck that I've used in DR presentations given over the last couple of months.

That finally brings the discussion around to Site Recovery Manager specifically. Jay goes over some of the features of SRM, and discusses some "do's and dont's" for SRM. For example, SRM isn't really intended to provide failover for a single VM, although you can architect it to do that (put that VM on a single LUN by itself and create a Protection Group for that LUN and VM, then craft your Recovery Plan).

It's important to note that SRM is **not** a replication product, but instead relies upon replication products from supported partners. This is done via the Storage Replication Adapter (SRA), a piece of software written by the storage vendor.

When setting up SRM, there are number of steps that it goes through. First, you have to integrate with the storage replication in place already (and yes, the storage replication needs to be in place already). Next, you need to map recovery resources; this creates the link between resources used in the Protected Site to resources that will be used in the Recovery Site. Third, you need to create Recovery Plans, which is the automated equivalent of the DR runbook. That is, the Recovery Plan defines which VMs will failover, in which order, at the Recovery Site. That's a bit of simplistic overview but it does get the point across.

At this point, I've decided that I'm going to try to get into a different session. I'm quite familiar with SRM, a lot of readers are probably familiar with it as well, and it doesn't look like there is anything new that will be revealed here. For those readers that aren't familiar with SRM, let me know in the comments. If there's enough interest, I'll write something separate after my return from VMworld 2008.
