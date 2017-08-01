---
author: slowe
categories: Liveblog
comments: true
date: 2007-09-13T11:29:52Z
slug: vmworld-2007-day-3-keynote-liveblog
tags:
- Virtualization
- VMware
- VMworld2007
title: VMworld 2007 Day 3 Keynote Liveblog
url: /2007/09/13/vmworld-2007-day-3-keynote-liveblog/
wordpress_id: 541
---

Here we are again back in the Moscone Center for the keynote of Day 3 of VMworld 2007. Unlike yesterday's keynote from Cisco's John Chambers, today's keynote is expected to be much more exciting and more revealing. (Make no mistake: John Chambers is a very skilled speaker, but my personal opinion is that yesterday's keynote was long on hype and short on reality.) Taking the stage today is VMware co-founder and Chief Scientist, Mendel Rosenblum. His [keynote last year][1] revealed VMware's work on capturing the live execution stream, a feature that made its way into VMware Workstation and will, presumably, also find its way into the enterprise platform as well. Expected this year is news about the next version of VMware Server. We shall see.

The session started with some video clips from various attendees here at the conference this year. My favorite clip was the tongue-in-cheek comment about snapshotting the conversation with the spouse, so you could roll back when you say something wrong. Come on, we've all done it---how many times have we said something we wish we could take back? (Also, I just have to toss my vote in: Karthik Rau does _not_ look like the guy from Heroes.)

Mendel started the keynote by taking a very high-level review of virtualization, talking about what virtualization is and how it works. We then move from talking about virtualization in abstract terms into talking about virtualization as it relates to VMware's Virtual Infrastructure. By adding this level of indirection (as it's known in computer science), Virtual Infrastructure enabled such things as consolidation (running multiple instances of a workload on a single piece of hardware), VMotion (live migration of workloads between physical hosts), and DRS (load balancing of workloads across groups of physical hosts). It also makes it far easier to add capacity, and the introduction of ESX Server 3i is supposed to further streamline that process.

In embracing these technologies, problems have cropped up. One of these problems is VMotion compatibility, and now hardware vendors are stepping up to help address this problem. AMD has Extended Migration and Intel has FlexMigration, which will enable the VMware software to erase VMotion boundaries. Mendel also officially introduced "Storage VMotion," which is essentially the reverse of VMotion. In Storage VMotion, the CPU execution remains on the same physical host but the virtual disks are being moved from one storage area to a different storage area. Mendel and Kit then proceeded to provide a demo of Storage VMotion. In the demo, they moved the disks for a running Oracle VM from one VMFS datastore to a different datastore.

Obviously, this functionality opens a number of new doors. As Mendel mentions, this greatly streamlines migrations for organizations that lease storage, helps us dynamically adjust the SAN utilization, etc. This is pretty powerful stuff.

The keynote next moves on to the idea of virtual appliances. One of the key issues with virtual appliances, however, is the distribution of virtual appliances, and Mendel discussed the idea of "streaming" virtual appliances down to clients, which they are referring to as "instant on". The idea here is that only those blocks that are absolutely necessary for the operation of the VM. Using a small executable and some prefetching technology, this allows VMware to have virtual appliances that can be utilized almost immediately, without having to wait for the entire appliance to download. While very valuable in using downloadable virtual appliances, one of the most exciting and potentially powerful use cases is the use of this technology in VDI scenarios. Recall that I predicted, based on last year's keynotes, that VDI would evolve to include check-out/check-in functionality, and it looks like this is the vehicle whereby this kind of functionality can be delivered.

Mendel next moved on to the idea of high availability in the datacenter. The idea of VMware HA allows us to recover VMs when a physical host fails. In his keynote, he alludes to the idea of detecting failures within the guest (the host was still running) and restarting it on another host. That's nice, but what is really powerful is what Mendel discusses next, and the idea of using record/replay as a high availability solution, known as "continuous availability." The idea here is that an execution stream for a VM is being captured and redirected to a secondary VM, live and in real-time. This creates two VMs in lockstep with each other. In the event of a hardware failure, the secondary VM will instantaneously fails over. (I suppose I don't need to tell anyone that this could make a good replication technology as well.) For the final demo this morning, they demonstrated the continuous availability functionality.

The demo involved a pair of virtualized Exchange servers with some clients running LoadSim to generate a load against the virtual Exchange servers. The setup created "mirrored VMs". As a demonstration of the technology, Mendel pulled the power plug on one of the ESX servers, and the secondary VM and clients barely even noticed the failure. Very nice technology!

What are the hard IT problems that VMware should address? Mendel challenged attendees to think about the advantages that the virtualization layer offers, and to think about how the virtualization layer can be used to create "checkbox simple" solutions to these hard IT problems.

In addition to driving the hardware more efficiently, Mendel also wants to be able to utilize power more efficiently as well, and he eluded to future functionality that might lend itself to more effective power management.

In summary, Mendel believes that the benefits of virtualization are only starting to be realized, and that VMware has only scratched the surface of the valuable functionality that the virtualization layer offers.

No announcements on VMware Server, though, which was expected by more than a few people.

[1]: {{< relref "2006-11-08-vmworld-2006-day-2-keynote.md" >}}
