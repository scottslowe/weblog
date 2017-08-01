---
author: slowe
categories: Information
comments: true
date: 2008-11-19T14:40:59Z
slug: virtualization-short-take-23
tags:
- ESX
- HyperV
- SAN
- Virtualization
- VMotion
- VMware
- Windows
title: 'Virtualization Short Take #23'
url: /2008/11/19/virtualization-short-take-23/
wordpress_id: 1039
---

A short while ago, I had a colleague in the blogging industry ask me if the reason I was writing "Short Takes" was because I was too busy to write in-depth articles. At the time, I told this colleague no, but now I'm wondering if I should change that answer...

In any event, here's my list of links and tidbits that I found interesting, amusing, or useful over the last week or so. Enjoy!

* Eric Sloof of NTPRO.NL [points out](http://www.ntpro.nl/blog/archives/759-IP-Storage-and-VM-swap-files.html) that VMware has updated their best practices to "allow" the placement of VM swap files on NFS, rather than recommending VMFS. Does anyone have a link to the VMware KB article with those updated recommendations?

* Edward Haletky points out a process for [performing "secure P2V" operations](http://itknowledgeexchange.techtarget.com/virtualization-pro/secure-method-to-p2v-across-security-zones/). Because the P2V process generally involves network communications with VirtualCenter and/or the VMware ESX Service Console, a straight P2V process would cross security boundaries. Edward's phased approach helps with that issue.

* Into PowerShell, but finding the process of modifying Offload Policies too difficult? [This should help out](http://www.peetersonline.nl/index.php/vmware/modifying-vswitch-offload-policy-with-powershell/) significantly.

* Expanding upon some earlier work, Rick has added "Resolution Paths" to [common network issues](http://www.vmwarewolf.com/common-network-issues-in-vmware-infrastructure/) and [common licensing issues](http://www.vmwarewolf.com/common-licensing-issues-in-vmware-infrastructure/). Excellent work!

* Leo provides some good information on [monitoring VI3 with Zenoss](http://lraikhman.blogsite.org/?p=346).

* Via [Alessandro](http://www.virtualization.info/2008/11/tool-hvremote.html) and [Ben](http://blogs.msdn.com/virtual_pc_guy/archive/2008/11/18/new-hvremote-tool.aspx), I learned about the HVRemote tool for automating the configuration steps for enabling remote management of Hyper-V. HVRemote was created by John Howard and more information is available [here](http://blogs.technet.com/jhoward/archive/2008/11/14/configure-hyper-v-remote-management-in-seconds.aspx).

* Symbolik shares his [experience in troubleshooting a PSoD](http://symbolik.wordpress.com/2008/11/15/esx-troubleshooting-psod-purple-screen-of-death/) (Purple Screen of Death) with VMware ESX. In his case, the issue turned out to be related to NICs, but it's nice to see that he was able to zero in on the issue and get it resolved.

* Larger installations with lots of LUNs and an active/active SAN may find [this script published by Duncan](http://www.yellow-bricks.com/2008/04/01/load-balancing-activeactive-sans/) very helpful. The problem that this script addressed underscores the need for robust multipathing support in VMware ESX such as that which will be allowed via the new vStorage APIs.

* Need more information on Enhanced VMotion Compatibility (EVC)? Both [Gabe](http://www.gabesvirtualworld.com/?p=101) and [Rich](http://vmetc.com/2008/11/16/enhanced-vmotion-compatibility-evc-%e2%80%93-intel-example/) have recently tackled the issue on their own blogs. If you're wondering what EVC is, the short answer is that it's a way to automate [this process][1] in a supported fashion.

That's it for this time around. Thanks for reading!

**UPDATE:** Eric Sloof responded about the updated recommendations regarding VM swap files. The new information was disclosed in session TA2784 at VMworld 2008. See slide 38. Thanks for the information, Eric!

[1]: {{< relref "2007-06-19-more-on-cpu-masking.md" >}}
