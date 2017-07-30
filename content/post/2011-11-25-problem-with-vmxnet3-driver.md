---
author: slowe
categories: Explanation
comments: true
date: 2011-11-25T17:01:32Z
slug: problem-with-vmxnet3-driver
tags:
- Microsoft
- Networking
- Virtualization
- VMware
- vSphere
- Windows
title: Problem with VMXNET3 Driver
url: /2011/11/25/problem-with-vmxnet3-driver/
wordpress_id: 2480
---

A colleague on the EMC vSpecialist team (many of you probably know [Chris Horn](http:/twitter.com/horn_chris)) sent me this information on an issue he'd encountered. Chris wanted me to share the information here in the event that it would help others avoid the time he's spent troubleshooting this issue.

What Chris has found is that there is a flaw in Windows Server 2008 and Windows 7 that causes "orphaned NICs" when using the VMXNET3 network driver. There appear to be three cases in which this problem appears:

1. When you deploy an OVF or OVA of Windows Server 2008 or Windows 7

2. When you clone a VM running Windows Server 2008 or Windows 7 (this also applies to deploying from a template)

3. When deploying a vApp within vCD that contains Windows Server 2008 or Windows 7 (this can cause quite a bit of chaos)

Up until now, there were two available workarounds that appeared to resolve this issue:

1. Use the Intel E1000 driver, which doesn't cause the same problem. However, it's unclear how well the E1000 performs with 10 Gigabit Ethernet uplinks.

2. Use a statically assigned MAC address, which of course doesn't scale very well in large (and/or dynamic) environments. This is also very difficult to do in vCloud Director (apparently even rising to the level of having to hack the vCD database).

It would appear that the behavior Chris describes is captured in [this VMware KB article](http://kb.vmware.com/kb/1020078). There is also a hotfix available in [this Microsoft KB article](http://support.microsoft.com/kb/2526142); Chris has tested this hotfix and indicates that it does indeed fix the problem. The referenced Microsoft KB article and the referenced VMware KB article also reference [this third Microsoft KB article](http://support.microsoft.com/kb/2344941), further leading me to believe that the articles are indeed related to the same underlying issue.

If you are deploying Windows Server 2008 and/or Windows 7-based VMs in your environment, you might want to take a look at the linked VMware KB and Microsoft KB articles to be sure that you don't run into the same sorts of problems Chris was experiencing.

Thanks Chris!
