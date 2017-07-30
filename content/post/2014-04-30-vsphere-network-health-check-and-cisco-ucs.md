---
author: slowe
categories: Information
comments: true
date: 2014-04-30T09:21:17Z
slug: vsphere-network-health-check-and-cisco-ucs
tags:
- Cisco
- Interoperability
- Networking
- UCS
- Virtualization
- VMware
- vSphere
title: vSphere Network Health Check and Cisco UCS
url: /2014/04/30/vsphere-network-health-check-and-cisco-ucs/
wordpress_id: 3444
---

Reader Brian Markussen---with whom I had the pleasure to speak at the Danish VMUG in Copenhagen earlier this month---brought to my attention an issue between VMware vSphere's health check feature and Cisco UCS when using Cisco's VIC cards. His findings, confirmed by VMware support and documented in [this KB article](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2034795), show that the health check feature doesn't work properly with Cisco UCS and the VIC cards.

Here's a quote from [the KB article](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2034795):

>The distributed switch network health check, including the VLAN, MTU, and teaming policy check can not function properly when there are hardware virtual NICs on the server platform. Examples of this include but are not limited to Broadcom Flex10 systems and Cisco UCS systems.

(Ignore the fact that "UCS systems" is redundant.)

According to Brian, a fix for this issue will be available in a future update to vSphere. In the meantime, there doesn't appear to be any workaround, so plan accordingly.
