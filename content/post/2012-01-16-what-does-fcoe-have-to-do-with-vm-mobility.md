---
author: slowe
categories: Rant
comments: true
date: 2012-01-16T09:00:00Z
slug: what-does-fcoe-have-to-do-with-vm-mobility
tags:
- FCoE
- Storage
- Virtualization
- VMware
title: What Does FCoE Have To Do With VM Mobility?
url: /2012/01/16/what-does-fcoe-have-to-do-with-vm-mobility/
wordpress_id: 2520
---

I recently read over Stephen Foskett's [Eight Unresolved Questions About FCoE](http://blog.fosketts.net/2012/01/05/unresolved-questions-fcoe/) article, in which he outlines eight topics that should be addressed/prioritized in order to make FCoE, in his words, "truly world-class".

One of these topics was virtual machine mobility. Here's what Stephen wrote under the heading "How Do You Handle Virtual Machine Mobility?":

>As I described recently, virtual machine mobility is a major technical challenge for existing networks. The VMware proposal, the VXLAN, seems to be gaining traction right now. But this is only a solution for data networking. How will FCoE SANs handle virtual machine mobility? This remains unresolved as far as I can tell, though Ethernet switch vendors have come up with their own answers. Brocade demonstrated just such a solution at Networking Field Day 2, and I know that others have answers as well. But will there be an interoperable industry solution?

My response to Stephen's question is pretty simple: VM mobility isn't a problem for FCoE. **VM mobility is a problem for all storage, regardless of protocol.**

In my view, tying VM mobility to FCoE is simply buzzword hype. Let's look at this from a technical perspective---what is so different about FCoE compared to FC, iSCSI, or NFS with regard to VM mobility? Nothing! Regardless of storage protocol, when you move a VM from one data center to another data center, the VM's storage is _still going to sit back in the original data center._ The protocol whereby we access that data doesn't change that fact.

Yes, you can route the IP-based protocols (iSCSI and NFS), which might give you some additional flexibility. But the migrated VM's storage is _still going to sit in the original data center_--the only thing you've accomplished is introduced some additional latency by adding routers into the mix.

As you can see, VM mobility and storage isn't an FCoE problem---it's a storage problem, plain and simple. Protocols can't fix it. What's needed to fix it is a fundamental shift in how storage is designed. We need distributed file systems (not just clustered file systems), distributed caching algorithms, greater integration between the block storage and the distributed file systems, greater hypervisor awareness of the underlying storage topology, and more efficient data movement mechanisms. Until then, we can rage all we want about how this protocol or that protocol doesn't address VM mobility, but we're simply missing the mark.
