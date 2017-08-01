---
author: slowe
categories: Information
comments: true
date: 2008-12-23T23:21:16Z
slug: random-reboots-with-vmware-esx-35-update-3
tags:
- ESX
- ESXi
- Virtualization
- VMotion
- VMware
- VMwareHA
title: Random Reboots with VMware ESX 3.5 Update 3
url: /2008/12/23/random-reboots-with-vmware-esx-35-update-3/
wordpress_id: 1110
---

I've been communicating with a reader who is experiencing random reboots of virtual machines on his HA/DRS-enabled cluster running VMware ESX 3.5 Update 3. At first, I thought his problem was related to the bug with VM failure monitoring that I [discussed here][1], but upon further discussion the random reboots are continuing to occur even when VM failure monitoring is disabled. The only relief the reader has been able to find thus far has been to completely disable VMware HA on his cluster, which---to be honest---is a less than acceptable solution.

After a little bit of digging around, I turned up [this VMware Communities thread](http://communities.vmware.com/thread/178417), in which several other users also indicate they are seeing the same kinds of problems. The thread closes out by referencing [this post by Duncan Epping](http://www.yellow-bricks.com/2008/12/12/vms-may-unexpectedly-reboot-when-using-vmware-ha-with-virtual-machine-monitoring/) regarding the VM failure monitoring bug. Clearly, though, this bug should not be affecting users who do not have VM failure monitoring enabled. I also found [this blog post](http://www.ivobeerens.nl/?p=180) about another user having the issue, although it sounds like his problem was solved by disabling VM failure monitoring.

Further research turned up [this KB article on a post-Update 3 patch](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1007501) that may address some of the random reboot issues. Judging from the KB article, it looks like the random reboots may be caused due to an unexpected interaction between VMotion and an option to automatically upgrade VMware Tools. This is just speculation, of course, but the symptoms seem to fit.

Have any other users out there experienced this problem? If so, what was the fix, if any? It sounds like there may be more to this issue than perhaps I first suspected.

[1]: {{< relref "2008-12-12-vmware-ha-problem-with-update-3.md" >}}
