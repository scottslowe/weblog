---
author: slowe
categories: Information
comments: true
date: 2010-08-28T13:16:21Z
slug: technology-short-take-2
tags:
- Cisco
- EMC
- HyperV
- Microsoft
- Nexus
- Security
- Storage
- UCS
- Virtualization
- VMware
- Networking
title: 'Technology Short Take #2'
url: /2010/08/28/technology-short-take-2/
wordpress_id: 2053
---

Welcome to Technology Short Take #2, a collection of links, thoughts, ideas, and items pertaining to data center technologies---virtualization, networking, storage, and security. I hope you find something useful or interesting!

* The release of FLARE 30 and DART 6 by EMC (formally announced last week) introduces some new concepts and new functionality. Matt Hensley recently did a write-up on some of the new functionality in this post on [virtual provisioning, storage pools, and FLARE 30](http://matthensley.wordpress.com/2010/08/23/virtual-provisioning-storage-pools-and-flare-30/). It's worth a read if you aren't already familiar with these technologies and need a primer.

* If you are looking for **the** definitive guide on connectivity between various VMware vSphere components and the TCP/UDP ports required, you need only look [here](http://www.vreference.com/2010/08/03/latest-firewall-diagram/). Great information!

* Here's a great guide from Cisco on deployment options when [deploying 10 Gigabit Ethernet on VMware vSphere 4.0](http://www.cisco.com/en/US/prod/collateral/switches/ps9441/ps9902/white_paper_c07-607716.html) with the Nexus 1000V or the VMware vNetwork Distributed Switch. I've read through it, but I've added it to my list of documents to go back and study more carefully; there's lots of useful information in here.

* Way back in March Dave Convery posted this article on [limitations with VMware vShield Zones](http://www.dailyhypervisor.com/2010/03/12/vshield-zones-some-serious-gotchas/). While re-reading that article today, I noted in the comments that the Nexus 1000V has a feature called Virtual Service Domains that help address some of the limitations of vShield Zones (at that time). As pointed out in the comments, this makes vShield Zones usable in two NIC scenarios such as with Cisco UCS. If anyone has any additional links on Virtual Service Domains, please share them in the comments. This is a topic that I think needs some additional attention.

* This article is a good breakdown of the [differences in storage identifiers](http://geeksilver.wordpress.com/2010/08/09/vmware-vsphere-4-1-vs-esx-3-x-storage-identifier-understanding/) between ESX 3.x and ESX 4.1.

* Jeff Woolsey at Microsoft finally wraps up his series of articles on Hyper-V Dynamic Memory with [Part 6](http://blogs.technet.com/b/virtualization/archive/2010/07/12/dynamic-memory-coming-to-hyper-v-part-6.aspx). I've been reading this series pretty faithfully as Jeff systematically lays out the various ways in which memory is handled in a virtualization scenario, and I've been consistently struck by the impression that Jeff was working really hard to distinguish what Microsoft was doing with Hyper-V from what VMware does with ESX/ESXi. In the end, though, I can't help but see all the similarities between the two. Dynamic Memory allocates additional memory to a VM as it needs it (much the same way ESX/ESXi does by allocating memory only as requested by the VM) and reclaims free pages from the VMs (just like ESX/ESXi reclaims idle pages via idle page reclamation). When under memory pressure, Hyper-V might force the guests to page out to disk; ESX/ESXi's memory balloon driver achieves the same effect. What's missing, obviously, is that with Hyper-V the hypervisor itself won't swap pages out to disk (ESX/ESXi will do this under extreme circumstances). Am I missing something, or is Microsoft's Dynamic Memory a lot more like VMware's memory management technologies than Microsoft wants to admit? Feel free to enlighten me (courteously and with full disclosure) in the comments if I'm missing something.

* Via [Geert Verbist's site](http://virtual.bist.be/?p=175), I found this article on [application consistent quiescing](http://vknowledge.wordpress.com/2010/08/18/application-consistent-quiescing-and-vdr/) via VMware's VSS integration in VMware Tools. (For more information on VSS support within VMware Tools, check out [my liveblog from Partner Exchange][1] earlier this year.) This is good to hear, but what's still not clear is whether the application consistent snapshots will truncate transaction logs. If anyone has more information, speak up in the comments.

* I think I pointed this out a week or two ago on Twitter, but I thought I'd mention here at well. If you ever need to help decode which WWPNs map to which ports on an EMC CLARiiON array, [this article](http://clariionblogs.blogspot.com/2007/11/storage-processor-ports-wwns.html) is quite helpful. Anyone have matching articles for EMC Symmetrix, NetApp, HP, HDS, or other arrays?

* With the formal announcement by VMware that vSphere 4.1 will be the last major release that includes ESX, ESXi is naturally getting much more attention. With that, there's been a flurry of ESXi-related articles:  

[Using vMA As Your ESXi Syslog Server](http://www.simonlong.co.uk/blog/2010/05/28/using-vma-as-your-esxi-syslog-server/)  

[The Migration From ESX to ESXi is Happening: Moving Configurations, Part 1](http://www.kendrickcoleman.com/index.php?/Tech-Blog/the-migration-from-esx-to-esxi-is-happening-moving-configurations.html)  

[The Migration from ESX to ESXi is Happening: Moving Configurations, Part II](http://www.kendrickcoleman.com/index.php?/Tech-Blog/the-migration-from-esx-to-esxi-is-happening-moving-configurations-part-ii.html)  

[My VMware ESXi Installation Checklist](http://kuparinen.org/martti/comp/vmware/esxichecklist.html)  

[Virtually Ghetto: ESXi 4.1 - Major Security Issue](http://www.virtuallyghetto.com/2010/07/esxi-41-major-security-issue.html) (also documented [here in the VMware KB](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1024500))  

[ESXi 4.1 - Major Security Issue - The Sequel and the Workaround](http://deinoscloud.wordpress.com/2010/07/18/esxi-4-1-major-security-issue-the-sequel-and-the-workaround/)  

[ESXi 4.1 Active Directory Integration](http://technodrone.blogspot.com/2010/07/esxi-41-active-directory-integration.html)

* If you're into Cisco UCS but like Hyper-V instead of VMware vSphere, Cisco has a [white paper on Cisco UCS with Hyper-V](http://www.cisco.com/en/US/docs/solutions/Enterprise/Data_Center/App_Networking/hypervexchange.html#wp274268) for delivery of virtualized Exchange 2010.

* I'm a command-line junkie, so I liked this article on [how to put an ESX host into maintenance mode](http://www.technicaljourney.com/2010/05/place-an-esx-host-into-maintenance-mode-using-the-command-line/) from the CLI.

* For those seeking to get up to speed on the Nexus 7000 switches, "Fryguy" posted [some training documents](http://fryguypa.wordpress.com/2010/07/05/nexus-7000-training-documentation/) on his site. I haven't read them (yet), but they're on my list of documents to read (a list that grows ever longer...)

I guess that will do it for this time around. I hope that you've found something useful and, as always, feel free to add more useful links or tidbits in the comments. Thanks for reading!

[1]: {{< relref "2010-02-09-partner-exchange-2010-session-techbc0320.md" >}}
