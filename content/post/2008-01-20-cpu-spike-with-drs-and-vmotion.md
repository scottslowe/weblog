---
author: slowe
categories: Explanation
comments: true
date: 2008-01-20T17:44:32Z
slug: cpu-spike-with-drs-and-vmotion
tags:
- ESX
- Virtualization
- VMotion
- VMware
title: CPU Spike with DRS and VMotion
url: /2008/01/20/cpu-spike-with-drs-and-vmotion/
wordpress_id: 610
---

An issue has been discovered that can cause a CPU spike when VMotion is used in a DRS-enabled cluster with ESX Server 3.5 and VirtualCenter 2.5. It is my understanding that this can occur with both DRS-initiated VMotion operations as well as manually-initiated VMotion operations.

Fortunately, there's a workaround for this issue which involves editing the `vpxd.cfg` on the VirtualCenter server. Full details on the change that needs to be made, as well as on the log entries that you should see on ESX Server afterward, are found [in this article](http://www.vmwarewolf.com/vmotion-in-35-drs-enabled-cluster-causes-guest-cpu-to-rise-dramatically/).

**UPDATE:** It appears that VMwarewolf has had to [pull the original article](http://www.vmwarewolf.com/crossing-the-line/) that described this problem and the workaround. Fortunately, Google has a long memory, and here's the workaround for this problem.

Immediately after the `<vpxd>` line in `vpxd.cfg`, add the following lines:

	<cluster>  
	<VMOverheadGrowthLimit>5</VMOverheadGrowthLimit>  
	</cluster>

I'm guessing that this information may not be information that VMware wants easily disseminated to the world, or that the workaround has not been fully and completely tested. So, use this information at your own risk.

In the meantime, [this Google search](http://www.google.com/search?hl=en&q=vmwarewolf+vpxd+CPU+spike&btnG=Google+Search) will return the now-unavailable page; use the [Cached](http://64.233.169.104/search?q=cache:l8o2KgswQPkJ:www.vmwarewolf.com/+vmwarewolf+vpxd+CPU+spike&hl=en&ct=clnk&cd=1&gl=us) link to see the workaround and the details from the log file that will help troubleshoot the problem.

**UPDATE 2:** VMware has now published [this KB article](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1003638) about the issue, along with the workaround for the problem.
