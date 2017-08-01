---
author: slowe
categories: Information
comments: true
date: 2010-05-25T11:35:07Z
slug: virtualization-short-take-41
tags:
- Cisco
- Citrix
- HyperV
- UCS
- VDI
- Virtualization
- VMware
- Xen
title: 'Virtualization Short Take #41'
url: /2010/05/25/virtualization-short-take-41/
wordpress_id: 1954
---

It has been a busy week since I last posted, and the blogging/micro-blogging world has been quite busy. I've gathered quite the collection of links and posts over the last week or so; here are a few that caught my eye. Welcome to Virtualization Short Take #41!

* About a month ago Rick Vanover posted a quick note about [the use of Disk.SchedNumReqOutstanding as a potential performance tweak](http://virtualizationreview.com/blogs/everyday-virtualization/2010/04/vsphere-performance-tweaks.aspx). As Rick mentions at the end of his article, it's important to "test before and after with an intensive workload"; otherwise, you could find yourself actually hurting performance. Like so many performance tweaks, it really depends upon your specific environment and your specific workloads. Definitely refer to some of the linked resources at the end of Rick's article ([Duncan's stuff](http://www.yellow-bricks.com/) is always helpful) for more details.

* Speaking of Duncan, his [post on vCPU limits](http://www.yellow-bricks.com/2010/05/18/limiting-your-vcpu/) is a great read and helps dispel a common misconception about vCPU limits. Remember the [definition of a hertz](http://www.merriam-webster.com/dictionary/hertz) folks--300 million cycles per second (300MHz) means 300 million cycles _per second_. It doesn't mean 500 million cycles per second, or 700 million cycles per second.

* Frank Denneman's article on [memory reservations and resource pools](http://frankdenneman.nl/2010/05/resource-pools-memory-reservations/) is also a really good read.

* Kenneth van Ditmarsch has a good post on [using datastore permissions](http://virtualkenneth.com/2010/05/18/vmfs-datastore-permissions-usefull-for-srm/) to help ensure that VMs are properly placed based on SLAs. This is the kind of operational advice that I think many organizations still need.

* Continuing our theme of resource allocation, here's a good post on the [effect of shares](http://www.jume.nl/articles/vmware/157-shares-low-normal-high).

* If you're interested in an early look at some of the features targeted for inclusion in VMware View 4.5, have a look at Matthijs Haverink's [post on View 4.5 expected features](http://virtualfuture.info/2010/05/vmware-view-4-5-expected-features/). If Matthijs' information is accurate, it looks like VMware has some good stuff planned.

* I had a URL in my Yojimbo collection for part 5 of the series on Hyper-V dynamic memory, but it doesn't seem to work anymore. I think the blog post was pulled. If anyone has a working link (yes, I've already checked Google), feel free to post it in the comments.

* Jeremy Waldrop of Varrow brings to light [a potential issue](http://jeremywaldrop.wordpress.com/2010/05/20/cisco-ucs-palo-and-emc-powerpath-ve-incompatibility/) with the Cisco "Palo" adapter (now called the Virtual Interface Controller, or VIC) and PowerPath/VE. There is a workaround that fixes the problem. It's important to note that the Cisco VIC isn't fully vetted or validated for Vblock yet; that's still in progress.

* As a follow up from my mention of this issue in [VST #40][1], I have more information on the Changed Block Tracking (CBT) issue. [This post from VMware](http://blogs.vmware.com/uptime/2010/05/changed-block-tracking-mismatch.html) has more information on the specific conditions needed to produce the problem. I have to say, it looks like a pretty specific set of circumstances. I'm curious to know your thoughts: is this a corner case, or a really significant problem? Personally, I'm leaning toward the former.

* EMC virtual appliances are really taking off; Chad [unwrapped the FMA virtual appliance](http://virtualgeek.typepad.com/virtual_geek/2010/05/get-yer-emc-fma-virtual-appliance-here.html) and fellow vSpecialist team member Nick Weaver [unveiled v2 of the "Uber" Celerra VSA](http://nickapedia.com/2010/05/19/besser-uber-celerra-vsa-uber-v2/) as well. I haven't had the chance to play with the FMA virtual appliance yet, but I'm traveling tonight so maybe I'll mess around with it on my laptop tonight from the hotel. (Yes, I'm a geek. What can I say?)

* Following Citrix's announcement of XenClient, their bare metal client hypervisor, and VMware's [response](http://blogs.vmware.com/view/2010/05/real-byoc-and-view-client.html) that perhaps the bare metal client hypervisor's use cases are more limited than many might think, Citrix has responded by [explaining XenClient to VMware](http://community.citrix.com/display/ocb/2010/05/18/Explaining+XenClient+to+Our+Friends+at+VMware). Bare metal hypervisors, unmanaged type 2 hypervisors, and policy-managed type 2 hypervisors all have value in the desktop virtualization space. Perhaps VMware should write a response to Citrix explaining the idea behind check-out/check-in of policy-controlled VMs? While I'm sure that I won't be very popular with VMware for saying this, I do have to agree with Citrix here: discounting the value of bare metal client hypervisors on the basis of a single use case is a bit disingenuous, especially when you've been promoting client hypervisors for a while.

* Looking to stay sharp and stay relevant in today's changing IT landscape? Mike DiPetrillo offers [some suggestions for skills](http://blogs.vmware.com/vcloud/2010/05/skills-needed-for-building-clouds.html) that IT folks should embrace.

* Kevin Goodman shared some information [here](http://blog.colovirt.com/2010/05/24/cisco-vmware-cisco-ucs-b6620-vmware-consolidation-ratio/) on consolidation ratios with his Cisco UCS environment. He admits he is constrained by RAM, which is common in many data centers today. There are two answers to that problem today: full-width UCS blades with support for massive amounts of RAM; or expensive, high-capacity RAM modules to drive memory capacity higher. It also looks like the Nehalem EX chipset is going to help address that problem with support for more memory buses and more memory slots. Once again I find it interesting that virtualization is helping to drive hardware development.

* Forbes Guthrie has published [v5 of his connections and ports diagram](http://www.vreference.com/2010/02/23/firewall-diagram-version-5/) for VMware ESX/ESXi. Definitely a useful resource!

* This [VMware KB article](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1020524) helps clarify the behavior of TPS with Intel Xeon 5500 (Nehalem)-based systems. This isn't new information (I believe Duncan might have pointed it out first?), but it's nice to see clarification of the behavior.

* OK, I'm probably showing my ignorance here (I haven't had the opportunity to spend as much time with View Manager as I would like), but who knew View Manager had [a command-line tool](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1019335)?

I guess that will wrap things up for this issue of Virtualization Short Takes. I hope you've found something useful!

[1]: {{< relref "2010-05-18-virtualization-short-take-40.md" >}}
