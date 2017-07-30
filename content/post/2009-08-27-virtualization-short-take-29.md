---
author: slowe
categories: Information
comments: true
date: 2009-08-27T19:18:19Z
slug: virtualization-short-take-29
tags:
- EMC
- iSCSI
- Microsoft
- NetApp
- Storage
- Virtualization
- VMware
- vSphere
- Networking
title: 'Virtualization Short Take #29'
url: /2009/08/27/virtualization-short-take-29/
wordpress_id: 1569
---

I wanted to go ahead and get another issue of Virtualization Short Takes out the door before VMworld, as I suspect that I'll be covered up both during and for some time after VMworld. So, here's my latest collection of links and articles about virtualization, storage, and anything else I find interesting.

* Chad Sakac brings up an important issue for EMC CLARiiON users also using vSphere and iSCSI; be sure to read [the full post](http://virtualgeek.typepad.com/virtual_geek/2009/08/important-note-for-all-emc-clariion-customers-using-iscsi-and-vsphere.html) for all the details. Basically, this bug in the FLARE code puts us back to using multiple IP subnets to scale iSCSI traffic. Bummer. I imagine they'll get it fixed up pretty quick, but until then it's back to the old way of scaling IP-based storage traffic. Chad's posts on VMware-storage integration ([Part 1](http://virtualgeek.typepad.com/virtual_geek/2009/07/state-of-the-art-integrating-storage-and-vmware-part-i.html) and [Part 2](http://virtualgeek.typepad.com/virtual_geek/2009/07/state-of-the-art-integrating-storage-and-vmware-part-ii.html)) are good reads as well.

* Nick Triantos weighs in with a good post on [how to configure ALUA support and Round Robin I/O in vSphere](http://blogs.netapp.com/storage_nuts_n_bolts/2009/08/vsphere-upgrading-from-non-alua-to-alua.html). This looks useful; too bad the old NetApp gear I have in the lab won't run the latest Data ONTAP version so I can test this myself. Oh, and you should also check out Nick's post on the [NetApp Collector and Analyzer for Virtual Environments](http://blogs.netapp.com/storage_nuts_n_bolts/2009/07/netapp-collector-analyzer-for-virtual-environments.html), which looks like it might be a handy tool for sizing NetApp storage environments.

* Duncan Epping points out a couple of issues related to VMFS block size in [this post on snapshots and block size](http://www.yellow-bricks.com/2009/08/24/vsphere-vm-snapshots-and-block-size/). Good find!

* Ben Armstrong puts up [a great post about competitive arguments](http://blogs.msdn.com/virtual_pc_guy/archive/2009/08/26/what-s-up-with-all-the-angst-mud-slinging.aspx). I have to say that I have a new respect for Ben after reading this post. He'd always presented himself very professionally, but his open approach to comparing virtualization products is very refreshing, and one that I wish more people would adopt. I'm particularly impressed that Ben quoted [Proverbs 27:17](http://www.biblegateway.com/passage/?search=Proverbs%2027:17&version=NIV) in his post.

* Aaron Sweemer [posted a newsletter from a co-worker](http://www.virtualinsanity.com/index.php/2009/08/26/notes-from-vmware-aka-mr-michael-whites-newsletter/) on his site that has some great information. You should definitely have a look, I think you'll find something useful there.

* Rick Scherer posted the steps necessary [to remove a rogue vCenter Chargeback plug-in](http://vmwaretips.com/wp/2009/08/21/vcenter-chargeback-uninstallation-rogue-plug-in/). Useful, but I wish all plug-ins provided a mechanism like this.

* Jason Nash brings to light [a bug in Cisco Nexus 1000V](http://jasonnash.wordpress.com/2009/08/24/a-small-bug-with-the-nexus-1000v/) when used in conjunction with CNAs. Be sure to have a look if this has any similarity to your environment. Like Jason, I have some Gen 1 Emulex CNAs so I may run into the same issue myself as I build out the Nexus hardware in the lab.

* The Systems Engineer (no name provided) gives a handy one-line command to [map ESX datastores to EMC CLARiiON LUNs](http://www.thesystemsengineer.com/uncategorized/esx-to-emc-clariion-lun-map/). I'll have to give this one a try once I get my CLARiiON up and running.

* Somewhere along the way I picked up the URL to this VMware KB article about [problems with iSCSI or NFS over an EtherChannel link](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1001109). Hmmm, that looks interesting, but when you read the article it points out that the issue exists when you are using EtherChannel but the vSwitch is configured as "Route based on originating virtual port ID." That's a configuration mismatch---of course you're going to have problems! Simply change the vSwitch to "Route based on ip hash" (the _strongly recommended_ setting when using EtherChannel) and the problems go away.

* Stevie Chambers (formerly of VMware, now with Cisco) posts about [10 technology advances since 2005](http://viewyonder.com/2009/08/20/intel-ucs-and-vsphere-8-advances-since-2005-and-why-they-matter/). The article is mostly about the Intel Xeon 5500 CPUs and a couple other features specific to Cisco's Unified Computing System (UCS); namely, the Palo adapter and the Catalina ASIC. While he wanders a bit, I think Stevie's point is about how virtualization architects and operations staff need to understand the impact of these technologies and how they affect the virtualization solution---a useful point, indeed.

* Paul Fazzone has a couple of great posts on the Cisco Nexus 1000V: first an article with an overview of [VM network security with the Nexus 1000V](http://faz1.com/blog/2009/08/17/vm-network-security-with-the-nexus-1000v/), then a second article describing [how the Nexus 1000V compares to multiple vSwitches](http://faz1.com/blog/2009/08/20/two-vswitches-are-better-than-1-right/). Both are good reads for people seeking a bit more information on deployment scenarios for the Nexus 1000V.

* Computerworld posted this article about [the 7 half-truths of virtualization](http://news.idg.no/cw/art.cfm?id=BE0E4E5B-1A64-6A71-CE1886CAA556F2B4). The underlying point behind all of these "half-truths" is that in order for an organization to _really_ reap the benefits of virtualization, that organization needs to change, to adapt, and to grow with the virtualization initiative. If you just virtualize and don't change anything else, your ROI will be limited at best. I particularly agree with #5: if you're investigating VDI for short-term cost savings, you're barking up the wrong tree.

* [This](http://www.infoblox.com/news/release.cfm?ID=144) is kind of cool. I might put this on my home network.

* I haven't had my chance to talk with Arista yet, but I'm surprised that there hasn't been more buzz around their announcement of [vEOS](http://www.aristanetworks.com/en/vEOS). In fact, I had to hear about it (other than a very brief e-mail from Doug Gourlay) from a Cisco contact! How crazy is that? I suppose, as I mentioned on Twitter, that Arista is going to make a big push next week during VMworld 2009 in San Francisco.

That wraps up this edition of Virtualization Short Takes. Next week will be a busy week; look for lots of coverage from the conference in San Francisco as well as summaries of my vendor meetings (and there are lots of them!). Until then, take care!
