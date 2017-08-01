---
author: slowe
categories: Liveblog
comments: true
date: 2007-09-12T16:36:47Z
slug: vmworld-2007-session-on-advanced-diagnostics-log-analysis
tags:
- ESX
- Virtualization
- VMware
- VMworld2007
title: VMworld 2007 Session on Advanced Diagnostics Log Analysis
url: /2007/09/12/vmworld-2007-session-on-advanced-diagnostics-log-analysis/
wordpress_id: 539
---

I signed up for this session (titled "Advanced ESX 3.0.x Diagnostics Log Analysis") with the hope of getting some in-depth, useful information on log analysis and troubleshooting. As a field person, out there in the trenches installing and supporting this stuff, having detailed information on the information that's being logged by the system is incredibly helpful. Let's hope that this session provides exactly that.

The presenter is Mostafa Khalil, a member of VMware Product Support Engineering. His accent isn't too bad, which is a pleasant surprise. He prefaces his session by saying that log analysis is a topic that could span days, but he was only given 60 minutes. Sounds like a way of saying, "I'm not going to cover everything you want me to cover."

We start out with a review of the ESX Server boot process, during which process Mostafa indicates that ESX no longer uses the Linux kernel to boot VMkernel. This is a fairly clear fact now with the release of ESX Server 3i, the embedded version of the hypervisor, but something to note nevertheless.

The easiest way to collect log files is to use the Virtual Infrastructure (VI) Client. Select an ESX server, go to the Administration menu, and select "Export Diagnostics Data". This will collect the following logs (all paths are relative to `/var/log`):

* vmkernel
* messages
* dmesg
* boot.log
* initrdlogs/*
* vmksummary
* vmware/hostd.log
* vmware/vpx/vpxa.log
* esxcfg-boot.log
* esxcg-firewall.log
* vmware-cim.log
* esxcfg-linuxnet.log
* esxupdate.log (see my [patch management session notes][1] for more on this one)
* oldconf/esx*.conf
* rpmpkgs
* vmkernel-version

Questions after the session appeared to indicate that using the vm-support script is equivalent to gathering logs through the VI Client. If there was anyone else from the session there, can you confirm that's what you heard as well?

Shortly after this point, my laptop battery died, and so I was unable to transcribe any more information. I will try to update this article later with some of the information I can remember. Sorry everyone! Feel free to chip in for a second laptop battery...

More posts are on the way, so keep reading, and keep the comments and corrections coming!

[1]: {{< relref "2007-09-12-vmworld-2007-session-on-esx-patch-management.md" >}}
