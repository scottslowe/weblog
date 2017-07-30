---
author: slowe
categories: News
comments: true
date: 2008-11-11T11:18:50Z
slug: recent-vmware-security-notice
tags:
- ESX
- ESXi
- Security
- Virtualization
- VMware
title: Recent VMware Security Notice
url: /2008/11/11/recent-vmware-security-notice/
wordpress_id: 1025
---

I started to mention this in Virtualization Short Take #22, but I felt that burying mention of a security notice amongst a bunch of other links just wasn't the right way to bring it to everyone's attention. I don't want to be accused of crying wolf, but I do want readers to be aware of this sort of issues when they arise.

Via [Infosecurity.us](http://infosecurity.us/?p=3222) and [Tarry Singh](http://tarrysingh.blogspot.com/2008/11/vmware-security-notice-cpu-flaw-may.html), I saw that VMware had released a security notification regarding a potential flaw in both the hosted products (VMware Workstation, Server, ACE, and Player) as well as ESX and ESXi. At the root of the issue is a potential flaw in the way that these products handle the Trap flag, and this potential flaw might lead to privilege escalation within the guest operating system. Yes, you read that right---a flaw within the _host_ could lead to privilege escalation in the _guest_.

The full VMware security advisory, [VMSA-2008-0018](http://www.vmware.com/security/advisories/VMSA-2008-0018.html) (incorrectly listed as VMSA-2009-0018), provides full details on the specific versions that are affected and provides links to applicable patches for vulnerable products. Interestingly enough, the latest versions of the hosted products---VMware Workstation 6.5, VMware Server 2.0, and VMware ACE 2.5--are not affected.

If you aren't keeping your VMware ESX hosts patched using Update Manager, now might be a good time to start.
