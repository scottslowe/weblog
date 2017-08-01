---
author: slowe
categories: News
comments: true
date: 2009-01-09T12:05:26Z
slug: marathon-to-bring-everrun-vm-to-hyper-v
tags:
- Citrix
- HyperV
- Microsoft
- Virtualization
- Xen
title: Marathon to Bring EverRun VM to Hyper-V
url: /2009/01/09/marathon-to-bring-everrun-vm-to-hyper-v/
wordpress_id: 1130
---

Today, Marathon Technologies announced an expanded development and marketing agreement with Microsoft Corporation designed to bring fault tolerance and high availability to a wider audience. As I understand it from talking to the folks at Marathon, the ultimate goal of this new agreement will bear fruit in two key areas:

1. First, Marathon will expand support for Windows Server 2008 on its existing EverRun VM solution for Citrix XenServer. Today, that product provides support for Windows Server 2003 on XenServer.

2. Second, EverRun VM will be supported with a future version of Hyper-V, bringing the same fault tolerance and high availability features available today with XenServer to Hyper-V.

Marathon and Citrix made some announcements about EverRun VM and XenServer HA around the timeframe of the [XenServer 5 announcement][1] at VMworld. I wrote [about that here][2] and [again here][3]. Have a look at those articles for more information about how EverRun VM functions today in a Citrix XenServer environment. It is anticipated that EverRun VM on Hyper-V will look and behave in exactly the same fashion as today's XenServer variant.

What Marathon is ultimately seeking to achieve is feature parity between EverRun VM on XenServer and EverRun VM on Hyper-V, as well as to provide a single management layer for manipulating fault tolerance and high availability regardless of the underlying virtualization layer. At some point, the idea of pairing XenServer and Hyper-V in a fault-tolerant pair might get explored, but that will strictly be driven by just how cozy Citrix and Microsoft become.

The Windows Server 2008 support for EverRun VM on XenServer---this is the ability to use EverRun VM to provide component-level or system-level fault tolerance for Windows Server 2008 guests---is expected in the first half of 2009. No dates were provided for a version of EverRun VM that will work with Windows Server 2008 and Hyper-V.

[1]: {{< relref "2008-09-15-citrix-announces-xenserver-5.md" >}}
[2]: {{< relref "2008-09-15-marathon-and-xenserver-ha.md" >}}
[3]: {{< relref "2008-09-26-everrun-vm-vmware-ha-and-vmware-ft.md" >}}
