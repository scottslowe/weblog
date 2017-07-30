---
author: slowe
categories: Rant
comments: true
date: 2008-01-07T16:43:16Z
slug: vmware-ha-clarification
tags:
- ESX
- Virtualization
- VMware
- VMwareHA
title: VMware HA Clarification?
url: /2008/01/07/vmware-ha-clarification/
wordpress_id: 601
---

VMwarewolf posted [an update](http://www.vmwarewolf.com/vmware-ha-admission-control/) today intended clarify the behavior of VMware HA admission control:

>I was previously under the impression that _Configured Memory_ (in a VM) was the number used in this consideration. Some further investigation has revealed this is incorrect. It is the _Reserved Memory_, plus overhead, that is used in this calculation.

If I'm reading this correctly, then reserved memory is the number that really matters when calculating failover capacity. That leads me to believe, assuming I am understanding this correctly, that a 2GB VM which only has 256MB of RAM reserved would be calculated as 340MB for the purposes of calculating failover capacity (256MB + 84MB overhead for a 32-bit virtual machine, slightly higher for a 64-bit virtual machine).

I suppose in a way that makes sense, since only reserved memory is ever really _"guaranteed"_ to a VM.

However, this again underscores the need for VMware to get on the ball and prepare some useful, comprehensive documentation about VMware HA, how it works, how it's configured, how one goes about troubleshooting it, and how it behaves when it's not configured correctly. Right now, the community is attempting to figure this out itself, and doesn't seem to be having a great deal of success.
