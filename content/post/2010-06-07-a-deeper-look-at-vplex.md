---
author: slowe
categories: Education
comments: true
date: 2010-06-07T11:00:00Z
slug: a-deeper-look-at-vplex
tags:
- EMC
- Storage
- Virtualization
- VPLEX
title: A Deeper Look at VPLEX
url: /2010/06/07/a-deeper-look-at-vplex/
wordpress_id: 1968
---

I've been promising a post on VPLEX for quite some time now, and now it's finally here. Having spent some hands-on time with VPLEX this past week, I think I'm finally ready to discuss VPLEX in some detail.

## The Basics

First, I need to cover the basics of what VPLEX is as well as what it isn't. VPLEX is another step in delivering EMC's vision of Virtual Storage and storage federation. There has been quite a bit of discussion over the difference between storage federation vs. storage virtualization (see [here](http://www.storagerap.com/2010/04/zeroing-in-on-a-definition-for-federated-storage.html) and [here](http://wikibon.org/blog/federated-storage-what-the-hell/) for two examples). Personally, like the phrase that Joe Kelly used in [a VPLEX post](http://blog.virtualtacit.com/home/2010/5/19/vplexing-their-way-to-distributed-data-centers-how-emc-stole.html) (emphasis mine):

>This is the ability to create a consistent view of a volume, independent of its location. **_This is the core behind Storage Federation._**

With this definition in mind, you can see that EMC has already delivered and will be delivering a number of technologies that support storage federation. Take sub-LUN FAST, for example; sub-LUN FAST presents a consistent view of a LUN regardless of the specific storage tier hosting the underlying blocks for that LUN. Blocks can (and will) be migrated automatically between tiers, yet the consistent view of the volume remains unchanged.

VPLEX accomplishes this definition of storage federation through in-band storage virtualization, which I personally think is why so many people are comparing it directly to IBM SVC, HDS USP-V, and NetApp V-Series. Yes, VPLEX does perform storage virtualization---but it's storage virtualization as part of delivering storage federation.

So, what is VPLEX exactly, then? Using in-band storage virtualization, VPLEX acts as a scale-out cluster delivering both local (within a data center) storage federation and metro (between data centers at synchronous distances) storage federation. A single VPLEX cluster can scale up to four engines; each engine contains two directors. Each director is equipped with loads of RAM (64GB of RAM, if I recall correctly), eight front-end 8Gb Fibre Channel ports, and eight back-end 8Gb Fibre Channel ports. This means a four-engine cluster offers 512GB of cache, 64 front-end 8Gb Fibre Channel ports, and 64 back-end 8Gb Fibre Channel ports. A single VPLEX cluster can support up to 8,000 virtualized LUNs.

VPLEX clusters can be combined into a metro-plex to provide storage federation between two data centers at synchronous data replication distances (less than 100km today). A metro-plex would consist of eight engines (sixteen directors), 1TB of cache, 128 front-end 8Gb Fibre Channel ports, and 128 back-end 8Gb Fibre Channel ports.

In addition to understanding what VPLEX is, it's also important to understand what VPLEX isn't. It's not a replacement for Invista, EMC's out-of-band storage virtualization solution. It's not a solution meant only for EMC arrays; VPLEX is also supported for non-EMC arrays, with support for more arrays in the works. And finally, it's not a VMware-only solution; VPLEX fully supports physical instances of Windows Server, Linux, Solaris, AIX, and other operating systems.

## Making it Real: Specifics in the Real World

If I were reading this post, I'd be asking myself right now,"OK, that's all great and wonderful, but what does it really _mean_?" I'm glad you asked.

Storage federation as provided by VPLEX means that the storage managed by VPLEX is active, read-writable storage across the entire VPLEX cluster or metro-plex (remember that a metro-plex is a pair of VPLEX clusters separated by synchronous replication distances). This means that if you have a VPLEX Local configuration with 2 engines, all the storage managed by this VPLEX Local cluster is read-writable across the entire cluster. Similarly, if you have a VPLEX Metro configuration with 4 engines (2 in each site), you can have storage that is read-writable in both locations _simultaneously_.

Consider a traditional storage replication solution: data exists in Site A and the array replicates the data to Site B. While the data is present at both sites, it's only writable at Site A. Site B is read-only. This is true of every replication solution of which I am aware on the market today. EMC's own replication products---like SRDF or RecoverPoint---behave this way. Sure, there are workarounds to that limitation, like image access with RecoverPoint. In the end, though, these are workarounds to the underlying replication model. VPLEX breaks that model by allowing you to have writable storage in Site A and Site B at the same time. The same LUN visible in two sites at the same time, writable in both locations.

Just think about that for a moment. You'll need a clustered file system to take advantage of this underlying storage functionality, but imagine something like Windows Server with Sanbolic's Melio FS to provide writable Windows LUNs in multiple sites at the same time. Of course, there's also the VMware use case where VPLEX provides writable access to a VMFS datastore between multiple data centers. Talk about making the hybrid cloud a reality---consider the use of VPLEX Metro between your on-site data center and a vCloud provider's data center. It would be the ultimate in workload mobility.

And those are just the VPLEX Metro examples. What about VPLEX Local? Ever had to migrate from one storage array to another storage array? Yes, you could use Storage vMotion. Or you could use VPLEX Local and not even have to get the VMware administrators involved---it would all happen under the covers. Think about being able to transparently migrate storage volumes among various arrays within your data center to meet the SLAs of the workload. Need Tier 1 storage? No problem, we'll use VPLEX Local to transparently migrate you to a VMAX. Don't need that level of performance or availability any more? No problem, we'll use VPLEX Local again to transparently migrate to a midrange storage platform.

Want to really freak your brain out? Think about VPLEX with sub-LUN FAST integrated into it...

I have so much more about VPLEX to share, but in the interest of keeping this already long blog post from getting even longer, I'll wrap it up here. Feel free to share your thoughts or questions about VPLEX in the comments below.
