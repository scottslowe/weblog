---
author: slowe
categories: Rant
comments: true
date: 2008-11-17T16:03:31Z
slug: fcoe-versus-mr-iovhuh
tags:
- Cisco
- FibreChannel
- iSCSI
- SAN
- Standards
- Storage
- Virtualization
title: FCoE versus MR-IOV...huh?
url: /2008/11/17/fcoe-versus-mr-iovhuh/
wordpress_id: 1028
---

I've had this link sitting in my "Articles To Read" list for quite some time, but---to be perfectly honest---I've been just too busy to really do anything about it. Now that a hectic few weeks has wrapped up and I have a small breather before the next hectic few weeks, I wanted to comment briefly on Doug Gourlay's [discussion of FCoE versus MR-IOV](http://blogs.cisco.com/datacenter/comments/fcoe_versus_pci_express_multi_root_iov_and_questionable_analysis/).

First, some background: For those that aren't familiar, FCoE is Fibre Channel over Ethernet, a T11 standard for running Fibre Channel Protocol over Ethernet, specifically 10 Gigabit Ethernet. More information on FCoE is found [here](http://www.fcoe.com/). MR-IOV is Multi-Root I/O Virtualization, a PCI SIG specification for using PCI Express (PCIe) to connect and share multiple devices. More information on MR-IOV can be found [here](http://www.pcisig.com/specifications/iov/multi-root/). MR-IOV is a multi-server extension to Single-Root I/O Virtualization, or SR-IOV.

Like Doug, I'll put in a disclaimer that I haven't read the report to which he's referring in his article, either. However, as an individual who has done some research on the topic of I/O virtualization, I will say that anyone who compares FCoE to MR-IOV is comparing apples to oranges. These two technologies, in my mind, are designed to address two different problems.

FCoE provides the ability to use a single physical transport--10 Gigabit Ethernet, in this case---for Fibre Channel Protocol (FCP) as well as TCP/IP, iSCSI, and other Ethernet-borne protocols. This allows for the creation of a unified fabric, a single physical transport that carries all the various kinds of traffic that Ethernet-based Local Area Networks (LANs) and Storage Area Networks (SANs) carry separately today. Via the IETF Converged Enhanced Ethernet (CEE) standard---adopted by Cisco as Data Center Ethernet&#8482;---FCoE will ultimately have the same low, predictable latency and error-free operation that FCP enjoys today. FCoE is _not_, however, designed or architected to do anything other than allow FCP to run over Ethernet. It's not intended to be a server interconnect technology. (Unless I'm missing something?)

MR-IOV, on the other hand, _is_ intended to play in the server interconnect field. Its purpose is not to allow FCP to run over Ethernet, or to allow FCoE, iSCSI, and other TCP/IP protocols share the same physical connections. MR-IOV's purpose is to allow multiple servers to share PCIe-based devices, like a FC Host Bus Adapter (HBA), or an iSCSI HBA, a 10 Gigabit Ethernet network interface card (NIC), or a video capture card. MR-IOV is intended to provide _I/O virtualization_, regardless of what type of I/O that might be. As long as the I/O runs across a PCI Express bus, MR-IOV comes into play.

I've heard multiple people refer to FCoE as an I/O virtualization technology, but I just don't agree. FCoE only applies to FCP over Ethernet. It doesn't apply to iSCSI. It doesn't apply to video traffic, or audio traffic, or HTTP traffic. It only applies to FCP over Ethernet. While I might allow that FCoE does allow for a form of virtualization, by virtualizing the physical transport beneath FCP, I would not call it I/O virtualization. Further, FCoE and MR-IOV are complementary. You could use MR-IOV to share a single Converged Network Adapter (CNA), which provides FCoE and 10 Gigabit Ethernet functionality, among multiple servers. In this situation, what's providing the I/O virtualization: MR-IOV, which is allowing multiple servers to use a single I/O card, or the CNA, which is putting the traffic onto the converged fabric?

I'm probably missing something huge here, some vital piece of information that would make sense why FCoE and MR-IOV would be considered competitive standards/specifications. Without that information, though, it just doesn't make any sense to me to compare these two different yet complementary technologies. Someone want to enlighten me?

**UPDATE:** I've corrected my use of "Data Center Ethernet" to Converged Enhanced Ethernet (CEE) when referring to the IETF standard. As correctly pointed out in the comments, Data Center Ethernet&#8482; is a Cisco trademarked term referring to their implementation of CEE.
