---
author: slowe
categories: News
comments: true
date: 2009-12-04T13:33:36Z
slug: multiport-uplink-driver
tags:
- Networking
- Virtualization
- VMware
- vSphere
title: Multiport Uplink Driver
url: /2009/12/04/multiport-uplink-driver/
wordpress_id: 1762
---

Virtual I/O is getting more attention. [This press release](http://www.chelsio.com/pr_120409.html) from Chelsio crossed my desk this morning:

>Virtual Multi-port Software allows for consolidation of switch ports and cabling by using 10Gb infrastructure while maintaining the existing Gigabit-based ESX setup.  The software enables the consolidation by keeping the infrastructure update completely transparent to the ESX hypervisor, enabling a 10Gb adapter to appear to the hypervisor as eight virtual Gigabit adapters.  By offloading the tasks performed by the hypervisor, the Chelsio adapters can deliver the best I/O performance for virtualized applications.

There's no mention of SR-IOV (more information on SR-IOV is available in [this post][1]), so I'm guessing that this is a proprietary technology similar to what HP is using in Virtual Connect Flex-10. The key difference with HP Virtual Connect Flex-10 and the Chelsio solution is that Flex-10 doesn't require _any_ software support in the OS or hypervisor, whereas Chelsio's solution does require software support (as does SR-IOV). Nevertheless, it's clear that I/O virtualization---even relatively simple forms of I/O virtualization such as this---is gaining more and more attention.

[1]: {{< relref "2009-12-02-what-is-sr-iov.md" >}}
