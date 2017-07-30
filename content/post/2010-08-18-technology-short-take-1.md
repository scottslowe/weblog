---
author: slowe
categories: Information
comments: true
date: 2010-08-18T10:37:14Z
slug: technology-short-take-1
tags:
- Networking
- Storage
- Virtualization
- vSphere
- VMware
title: 'Technology Short Take #1'
url: /2010/08/18/technology-short-take-1/
wordpress_id: 2015
---

I've decided to discontinue the Virtualization Short Takes series and replace it with a new series that incorporates items from the networking and storage realms as well. In truth, I was already doing a little bit of that anyway, but this will expand and formalize the coverage a bit. I'd love to hear your feedback on this new series, to help me ensure that I'm providing useful and pertinent information and to help me ensure that I'm presenting it in the most efficient way. So, please speak up in the comments if you have any suggestions for improvement. Thanks!

And with that brief introduction, welcome to Technology Short Take #1!

* You can tell just how far behind I've gotten with this sort of post when one of the items I'm mentioning was published almost three months ago. Still, one phrase in this article by Frank Denneman on [VM memory overhead](http://frankdenneman.nl/2010/05/virtual-machine-memory-overhead/) really caught my eye:  

	>Please be aware of the fact that memory overheads are growing with each new release of ESX, so keep this in mind when upgrading to a new version.

	What does this mean? Why are VM memory overheads increasing, and is this something to be concerned over? I can certainly understand the need to add features, but is the drive to add features to the hypervisor also driving bloat and making it less efficient in some aspects?

* Duncan Epping wrote a great post about a month ago on new behavior in vSphere 4.1 with respect to [HA/DRS and flattened shares](http://www.yellow-bricks.com/2010/07/22/6283/). This behavior is yet another reason you want to make sure that you really understand what you're doing when you assign reservations, limits, and shares to your VMs and resource pools. Both Duncan and his cohort-in-crime Frank, along with a few others, have been publishing lots of resource allocation-related posts:  

	[Which host is selected for an HA initiated restart?](http://www.yellow-bricks.com/2010/06/16/which-host-is-selected-for-an-ha-initiated-restart/)  

	[Memory reclamation, when and how?](http://frankdenneman.nl/2010/06/memory-reclaimation-when-and-how/)  

	[Reservations and CPU scheduling](http://frankdenneman.nl/2010/06/reservations-and-cpu-scheduling/)  

	[The Math Behind the DRS Stars](http://professionalvmware.com/2010/06/the-math-behind-the-drs-stars/)

* Need to flip the paths on your datastores over to a different HBA so you can perform some SAN maintenance? Leo Raikhman has a couple of scripts that can help with [performing SAN fabric maintenance on ESX](http://blog.core-it.com.au/?p=595).

* Larry Touchette at NetApp has a good [write-up on DRS host affinity rules](http://blogs.netapp.com/virtualization/2010/07/drs-host-affinity-in-vsphere-41.html) in vSphere 4.1. I have to take exception to Larry's tying of DRS host affinity rules to NetApp MetroCluster (DRS host affinity rules have so many more applications than just stretched clusters!), but that's a minor nit. For more information on DRS host affinity rules, you can also check out [this post](http://frankdenneman.nl/2010/07/vm-to-hosts-affinity-rule/).

* [This VMware KB article](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1020893) looks like it could be pretty handy if you are using the Nexus 1000V and need to migrate to a new vCenter Server instance.

* Dave Alexander does some "thinking out loud" with a post on [private VSANs](http://www.unifiedcomputingblog.com/?p=158). In the post he asks why we don't use PVSANs in the FC world in the same way we use PVLANs in the networking world. It's an interesting thought, but it sounds like all it accomplishes is simplifying the creation of zones. Surely there are easier ways to simplify the creation of zones than creating an administrative construct like PVSANs.

* More "thinking out loud" comes from Didier Pironet in his post on [enhancements to VMware DRS/HA/FT](http://deinoscloud.wordpress.com/2010/05/24/what-to-do-to-enhance-vmware-drshaft/). In his post, Didier suggests the incorporation of hardware information into DRS/HA/FT to allow for the creation of policy driven decisions that help decide where to place VMs based on hardware attributes. This is a useful line of thought and one that I think vendors will need to explore in order to allow service providers to offer various differentiated levels of service.

* I found [this brief post](http://packetlife.net/blog/2010/jul/15/emerging-terminology-lisp-and-trill/) quite helpful, as I was quite confused as to how the programming language had anything to do with routing. Turns out I was using the wrong acronym, and LISP instead stands for Location and Identity Separation Protocol. Ah, that makes a lot more sense now!

* Aside from a brief mention [here](http://www.latogalabs.com/2010/07/vmware-releases-vsphere-41/), I hadn't seen any references to the fact that VMware released [VMware Studio 2.1](http://www.vmware.com/support/developer/studio/studio21/release_notes.html) along with vSphere 4.1.

* Fellow EMC vSpecialist Simon Seagrave [recently published](http://www.techhead.co.uk/vmware-esx-i-moved-it-or-i-copied-it-whats-the-difference) a good article on when/how UUIDs are generated for virtual machines.

Because I'm so far behind, I have a _ton_ of other links that I've been collecting. They are all good articles and worth reading if you can spare the time. Here are a few that stand out:

[How does "das.maxvmrestartcount" work?](http://www.yellow-bricks.com/2010/06/30/how-does-das-maxvmrestartcount-work/)  

[Yes, You Really *Can* Run Tier-1 Enterprise Applications on vSphere](http://blogs.vmware.com/oracle/2010/06/yes-you-really-can-run-tier1-enterprise-applications-on-vsphere.html)  

[How to Configure Likewise "Open" Integration on vMA](http://www.virtuallyghetto.com/2010/06/how-to-configure-likewise-open-ad.html)  

[Got Network I/O Control?](http://blogs.vmware.com/networking/2010/07/got-network-io-control.html)  

[VMware HA Isolation Response Bug](http://thickclouds.com/2010/06/29/vmware-ha-isolation-response-bug/)  

[Cisco, VMware: Nexus 1000V Tracking VM Interface Errors](http://blog.colovirt.com/2010/06/07/cisco-vmware-nexus-1000v-tracking-vm-interface-errors/)

I guess that's going to do it for this time around. As I said earlier, I'd love to hear your feedback, positive or negative, as well as any suggestions for improvement. Thanks for reading!
