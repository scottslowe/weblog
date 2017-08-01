---
author: slowe
categories: Information
comments: true
date: 2008-06-12T11:48:49Z
slug: significant-networking-problem-with-hyper-v
tags:
- HyperV
- Microsoft
- Networking
- TechEd2008
- Virtualization
- Windows
title: Significant Networking Problem with Hyper-V
url: /2008/06/12/significant-networking-problem-with-hyper-v/
wordpress_id: 739
---

After the conclusion of VIR358, I went up to the front to speak with the presenters about the question I had during the session: what about NIC bonding or NIC teaming? You'll recall that I wondered about that [during the VIR358 session][1].

Well, it turns out that Hyper-V _does not support any form of NIC teaming or NIC bonding._ Yes, you read that right: you can't link more than one NIC to a virtual switch in Hyper-V.

If you follow my [del.icio.us linkstream](http://del.icio.us/slowe), you will probably have noted that I recently bookmarked a Microsoft KB article that describes how using [HP's Network Utility can cause Hyper-V to stop responding](http://support.microsoft.com/default.aspx/kb/950792). I guess this just goes to further support Hyper-V's lack of support for NIC teaming or bonding.

In my opinion, that is a **_huge_** problem. How does one go about providing network link redundancy to guests hosted on Hyper-V? Surely using Failover Clustering and Quick Migration isn't the answer here, is it? One of the presenters offered to get back to me with more information; I've already sent him an e-mail so he has my contact information. As soon as I hear something back, I'll be sure to update this post.

[1]: {{< relref "2008-06-12-vir358-hyper-v-architecture-scenarios-and-networking.md" >}}
