---
author: slowe
categories: Information
comments: true
date: 2013-09-05T09:00:00Z
slug: vendor-meetings-at-vmworld-2013
tags:
- Encryption
- FCoE
- Hardware
- Networking
- Security
- Storage
- VMworld2013
title: Vendor Meetings at VMworld 2013
url: /2013/09/05/vendor-meetings-at-vmworld-2013/
wordpress_id: 3269
---

This year at VMworld, I wasn't in any of the breakout sessions because employees aren't allowed to register for breakout sessions in advance; we have to wait in the standby line to see if we can get in at the last minute. So, I decided to meet with some vendors that seemed interesting and get some additional information on their products. Here's the write-up on some of the vendor meetings I've attended while in San Francisco.

## Jeda Networks

I've mentioned [Jeda Networks](http://jedanetworks.com/) before (see [here][1]), and I was pretty excited to have the opportunity to sit down with a couple of guys from Jeda to get more details on what they're doing. Jeda Networks describes themselves as a "software-defined storage networking" company. Given my previous role at EMC (involved in storage) and my current role at VMware focused on network virtualization (which encompasses SDN), I was quite curious.

Basically, what Jeda Networks does is create a software-based FCoE overlay on an existing Ethernet network. Jeda accomplishes this by actually programming the physical Ethernet switches (they have a series of plug-ins for the various vendors and product lines; adding a new switch just means adding a new plug-in). In the future, when OpenFlow or its derivatives become more ubiquitous, I could see using those control plane technologies to accomplish the same task. It's a fascinating idea, though I question how valuable a software-based FCoE overlay is in a world that seems to be rapidly moving everything to IP. Even so, I'm going to keep an eye on Jeda to see how things progress.

## Diablo Technologies

Diablo was a new company to me; I hadn't heard of them before their PR firm contacted me about a meeting while at VMworld. Diablo has created what they call [Memory Channel Storage](http://www.diablo-technologies.com/products/mcs.html), which puts NAND flash on a DIMM. Basically, it makes high-capacity flash storage accessible via the CPU's memory bus. To take advantage of high-capacity flash in the memory bus, Diablo supplies drivers for all the major operating systems (OSes), including ESXi, and what this driver does is modify the way that page swaps are handled. Instead of page swaps moving data from memory to disk---as would be the case in a traditional virtual memory system---the page swaps happen between DRAM on the memory bus and Diablo's flash on the memory bus. This means that page swaps are _extremely_ fast (on the level of microseconds, not the milliseconds typically seen with disks).

To use the extra capacity, then, administrators must essentially "overcommit" their hosts. Say your hosts had 64GB of (traditional) RAM installed, but 2TB of Diablo's DIMM-based flash installed. You'd then allocate 2TB of memory to VMs, and the hypervisor would swap pages at extremely high speed between the DRAM and the DIMM-based flash. At that point, the system DRAM almost looks like another level of cache.

This "overcommitment" technique could have some negative effects on existing monitoring systems that are unaware of the underlying hardware configuration. Memory utilization would essentially run at 100% constantly, though the speed of the DIMM-based flash on the memory bus would mean you wouldn't take a performance hit.

In the future, Diablo is looking for ways to make their DIMM-based flash appear to an OS as addressable memory, so that the OS would just see 3.2TB (or whatever) of RAM, and access it accordingly. There are a number of technical challenges there, not the least of which is ensuring proper latency and performance characteristics. If they can resolve these technical challenges, we could be looking at a very different landscape in the near future. Consider the effects of cost-effective servers with 3TB (or more) of RAM installed. What effect might that have on modern data centers?

## HyTrust

[HyTrust](http://www.hytrust.com) is a company with whom I've been in contact for several years now (since early 2009). Although HyTrust has been profitable for some time now, they recently [announced](http://www.businesswire.com/news/home/20130826005383/en/HyTrust-Raises-18.5-Million-Series-Funding-Accelerate) a new round of funding intended to help accelerate their growth (though they're already on track to quadruple sales this year). I chatted with Eric Chiu, President and founder of HyTrust, and we talked about a number of areas. I was interested to learn that HyTrust had officially productized a proof-of-concept from 2010 leveraging Intel's TPM/TXT functionality to perform attestation of ESXi hypervisors (this basically means that HyTrust can verify the integrity of the hypervisor as a trusted platform). They also recently introduced "two man" support; that is, support for actions to be approved or denied by a second party. For example, an administrator might try to delete a VM, but that deletion would need to be approved by a second party before it is allowed to proceed. HyTrust also continues to explore other integration points with related technologies, such as OpenStack, NSX, physical networking gear, and converged infrastructure. Be sure to keep an eye on HyTrust---I think they're going to be doing some pretty cool things in the near future.

## Vormetric

[Vormetric](http://www.vormetric.com) interested me because they offer a data encryption product, and I was interested to see how---if at all---they integrated with VMware vSphere. It turns out they don't integrate with vSphere at all, as their product is really more tightly integrated at the OS level. For example, their product runs natively as an agent/daemon/service on various UNIX platforms, various Linux distributions, and all recent versions of Windows Server. This gives them very fine-grained control over data access. Given their focus is on "protecting the data," this makes sense. Vormetric also offers a few related products, like a key management solution and a certificate management solution.

## SimpliVity

[SimpliVity](http://www.simplivity.com) is one of a number of vendors touting "hyperconvergence," which---as far as I can tell---basically means putting storage and compute together on the same node. (If there is a better definition, please let me know.) In that regard, they could be considered similar to Nutanix. I chatted with one of the designers of the SimpliVity OmniCube. SimpliVity leverages VM-based storage controllers that leverage VMDirectPath for accelerated access to the underlying hardware, and present that underlying hardware back to the ESXi nodes as NFS storage. Their file system---developed during the 3 years they spent in stealth mode---abstracts away the hardware so that adding OmniCubes means adding both capacity and I/O (as well as compute). They use inline deduplication not only to reduce storage capacity, but especially to avoid having to write I/Os to the storage in the first place. (Capacity isn't usually the issue; I/Os are typically the issue.) SimpliVity's file system enables fast backups and fast clones; although they didn't elaborate, I would assume they are using a pointer-based system (perhaps even an optimized content-addressed storage [CAS] model) that keeps them from having to copy large amounts of data around the system. This is what enables them to do global deduplication, backups from any system to any other system, and restores from any system to any other system (system here referring to an OmniCube).

In any case, SimpliVity looks very interesting due to its feature set. It will be interesting to see how they develop and mature.

## SanDisk FlashSoft

This was probably one of the more fascinating meetings I had at the conference. [SanDisk FlashSoft](http://www.sandisk.com/products/flashsoft/) is a flash-based caching product that supports various OSes, including an in-kernel driver for ESXi. What made this product interesting was that SanDisk brought out one of the key architects behind the solution, who went through their design philosophy and the decisions they'd made in their architecture in great detail. It was a highly entertaining discussion.

More than just entertaining, though, it was really informative. FlashSoft aims to keep their caching layer as full of dirty data as possible, rather than seeking to flush dirty data right away. The advantage this offers is that if another change to that data comes, FlashSoft can discard the earlier change and only keep the latest change---thus eliminating I/Os to the back-end disks entirely. Further, by keeping as much data in their caching layer as possible, FlashSoft has a better ability to coalesce I/Os to the back-end, further reducing the I/O load. FlashSoft supports both write-through and write-back models, and leverages a cache coherency/consistency model that allows them to support write-back with VM migration without having to leverage the network (and without having to incur the CPU overhead that comes with copying data across the network). I very much enjoyed learning more about FlashSoft's product and architecture. It's just a shame that I don't have any SSDs in my home lab that would benefit from FlashSoft.

## SwiftStack

My last meeting of the week was with a couple folks from [SwiftStack](http://www.swiftstack.com). We sat down to chat about Swift, SwiftStack, and object storage, and discussed how they are seeing the adoption of Swift in lots of areas---not just with OpenStack, either. That seems to be a pretty common misconception (that OpenStack is required to use Swift). SwiftStack is working on some nice enhancements to Swift that hopefully will show up soon, including erasure coding support and greater policy support.

## Summary and Wrap-Up

I really appreciate the time that each company took to meet with me and share the details of their particular solution. One key takeaway for me was that there is still lots of room for innovation. Very cool stuff is ahead of us---it's an exciting time to be in technology!

[1]: {{< relref "2013-06-04-technology-short-take-33.md" >}}
