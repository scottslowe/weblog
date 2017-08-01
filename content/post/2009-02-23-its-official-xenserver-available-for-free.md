---
author: slowe
categories: News
comments: true
date: 2009-02-23T12:00:24Z
slug: its-official-xenserver-available-for-free
tags:
- Citrix
- Virtualization
- Xen
title: 'It''s Official: XenServer Available for Free'
url: /2009/02/23/its-official-xenserver-available-for-free/
wordpress_id: 1207
---

Although word of this announcement [was leaked last week](http://practical-tech.com/infrastructure/citrix-to-offer-free-xenserver-virtualization/), today Citrix made the announcement official: Citrix XenServer will be available to customers free of charge. Although I was briefed by Citrix on this news the day before [word broke](http://www.virtualization.info/2009/02/citrix-to-release-xenserver-for-free.html), I assured Citrix that I would honor my word and not discuss the news until the embargo expired.

As I understand it, there are actually three parts to the announcement:

1. XenServer 5 will be available free. According to the information shared with me by Citrix, this won't be a stripped down version of XenServer, either: this is supposedly the equivalent of XenServer 5 Enterprise Edition, so functionality like live migration, Active Directory integration, and a centralized management console are all included.

2. Citrix will be unveiling a new product line called Citrix Essentials for XenServer and Hyper-V. This is a management product that provides automated lab management, dynamic provisioning, StorageLink&#8482; technology to leverage advanced storage functionality, and advanced high availability (but only when used with XenServer). Citrix Essentials will manage both Hyper-V and XenServer, and is intended to integrate with Microsoft System Center and other high-level management frameworks.

3. Finally, Citrix is extending their relationship with Microsoft around virtualization. Microsoft will be recommending Citrix Essentials for Hyper-V as the management platform, and Microsoft will be adding support for XenServer in System Center.

In addition to these moves, it is my understanding that Citrix will be more aggressively moving more of their XenServer-related code into the open source community. The first of these moves has already [been announced][1], and I am assured by Simon Crosby that more code is slated to be open sourced in the coming weeks and months.

So what's my take on this announcement? I've already stated that I'm glad to see Citrix giving back to the Xen open source community, from which they've benefited so greatly, and I'm looking forward to the continued movement of XenServer-related code into the open source community. The StorageLinkTM stuff looks pretty cool, and if Citrix can actually deliver on it then they'll be back on par with what's slated to be in the vStorage APIs from VMware. (Wonder who will deliver first?) Citrix Essentials looks interesting, but it's really too early to tell just yet. The cross-platform approach is nice, and a welcome acknowledgement of the heterogeneous nature of the data center, but as a new product it will take some time to establish itself in the market.

That leaves only free XenServer. There's no question that XenServer receives far less attention than Hyper-V, despite the fact that it's been around far longer than its Microsoft cousin. Now that XenServer is available at no charge, this will open the door of many more customers who may not have previously considered the Citrix product. That will likely increase uptake of the product in the SMB market, but I doubt that it will make a significant impact in the large enterprise market, where VMware's flagship products still rule. The other effect of the announcement will be on other Xen-based virtualization solutions, like Virtual Iron, who now have to compete with Citrix on a completely different playing field. I would not be surprised to see a number of these smaller players exit the game, either by acquisition or bankruptcy.

The real question here is this: what will VMware do? VMware continues to pour R&D dollars into the development of its hypervisor and surrounding applications, and only makes limited versions of its bare metal product available for free (ESXi). Will this push VMware to make "full" ESXi available for free? Or will VMware continue to believe, as they have in the past, that the hypervisor is still not a commodity, and therefore continue to charge for it? I suspect that their path will be the latter and not the former, and it will be justified by the presence of features like Storage VMotion and VMware FT that have yet to be replicated by any other vendor. While I have my concerns about that approach, only time will tell if my concerns are justified.

**UPDATE:** The Citrix press release is available [here](http://www.citrix.com/English/NE/news/news.asp?newsID=1687130).

**UPDATE 2:** As [shared by David Marshall](http://vmblog.com/archive/2009/02/23/vmlogix-adds-application-lifecycle-management-to-citrix-virtualization-solutions.aspx), it turns out that the automated lab management functionality of Citrix Essentials is being OEM'ed from VMLogix.

[1]: {{< relref "2009-02-19-citrix-open-sources-their-vhd-implementation.md" >}}
