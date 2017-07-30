---
author: slowe
categories: Information
comments: true
date: 2009-09-02T13:57:30Z
slug: notes-on-some-vmworld-vendor-meetings
tags:
- Storage
- Virtualization
- VMware
- VMworld2009
title: Notes on Some VMworld Vendor Meetings
url: /2009/09/02/notes-on-some-vmworld-vendor-meetings/
wordpress_id: 1637
---

I've had the opportunity to speak with a few different vendors over the last couple of days here in San Francisco at VMworld 2009. Here are some notes on my meetings.

## Virsto

My first meeting of the week was with [Virsto](http://www.virsto.com/), a early storage startup (they just closed Series A funding in the last few weeks). Virsto is led by some long-time storage professionals from companies such as StorageTek, Veritas, and others.

Virsto is unique, to me, in that they have an interesting view of the storage component. I met with Alex, one of the founders, and he used a term that I found quite illustrative and useful: "the I/O blender". This is the term he applied to the effect that the hypervisor has on I/O as it moves from the virtual server to the physical server to the storage layer. If you think about it, it makes sense: I/O from each virtual server has to be multiplexed onto the same HBAs as the I/O from every other virtual server. The end result is, of course, that the storage array ends up having to deal with small, random I/O workloads instead of large, sequential workloads. This impairs performance.

The Virsto solution combines a software portion that is currently architected only for Microsoft Hyper-V. Virsto's software component illustrates both the strength and the weakness of Hyper-V's indirect I/O model. It's a strength in that it's very easy to write a filter driver to run in the management partition to modify VM I/O; the weakness is that it's really easy to write a filter driver to run in the management partition to modify VM I/O. I'm being partially facetious here, but I hope you get the point. In any case, what Virsto's software layer does is help undo the I/O blender effect by working in conjunction with a storage staging layer. Typically this would be some sort of high-speed local storage, such as an SSD. As a result of the software working in conjunction with the hardware, Virsto can "re-assemble" I/O into workloads that are better suited for performance at the physical layer and thus undo the I/O blender effect.

Virsto's solution also allows for some forms of storage virtualization, in that different types of underlying block storage can be combined and managed by Virsto. Virsto's solution also offers snapshots (checkpoints), the ability to split data streams for replication, and better support for disk-to-disk (D2D) backups via their snapshots.

My biggest concern with Virsto is that they are competing in a space with lots of very large, very well-funded organizations that are laser-focused on making their storage work extremely well with VMware vSphere and virtual environments, including Hyper-V. Think of NetApp integration with Hyper-V, or EMC integration with Hyper-V (remember that Virsto supports only Hyper-V at this time). These companies have lots of development talent, lots of money, and an established presence. I fear it will be difficult for Virsto to really gain a foothold in that space.

## Xangati

[Xangati](http://www.xangati.com/) (pronounced "zan-gotti") is an application performance solution. I had a spirited discussion with the Xangati folks about what differentiates them versus other solutions like AppSpeed, BlueStripe, etc., in that Xangati relies upon network traffic information to measure application performance. In Xangati's case, they rely upon NetFlow (or the various vendor-specific implementations of NetFlow). At first, I found this a bit limiting because I wasn't aware that NetFlow v5 was supported in ESX 3.5 on vNetwork Standard Switches (I know, this is probably something everyone knows). But it indeed is (see [here](http://www.vmware.com/resources/techresources/1014)); the real question is whether it will continue to be supported on vNetwork Standard Switches on vSphere. In any case, Xangati insists that using NetFlow to gather network information is very different (and yields different results) than performing packet analysis. I must admit that I don't fully see the difference; perhaps a network guru can explain it?

Having stated all that, what Xangati does it pretty interesting. It requires that you enable NetFlow throughout the environment---on both virtual and physical switches and other physical network equipment---and then allows you to see end-to-end network usage on an application-by-application basis. From there, Xangati allows the organization to take "network recordings" of the network behavior and then replay that recording later. Different views can be created for different roles within the organization, allowing IT pros to see only the information they need or want to see.

I'll go back to my earlier statements and say that while Xangati offers some unique functionality---such as the ability for an end-user to initiate a network recording and submit that as a "Visual Trouble Ticket" to the help desk---I'm still at a loss to explain how, in the end, they are different from AppSpeed, BlueStripe, and others who provide end-to-end application performance and correlation. Yes, they use a different way (perhaps a superior way) of gathering information, but what the customer ultimately wants to know is this: "Which part is slow?" It seems that there are other, more well-established solutions already on the market that are trying to address this. Whether or not Xangati is successful in the space is yet to be seen, but I do wish them the best of luck!

I also met with Virtual Instruments and had a great discussion with them, but as I'm running a bit short on time I'll have to do their write-up later on. Check back here later for the write-up on Virtual Instruments.
