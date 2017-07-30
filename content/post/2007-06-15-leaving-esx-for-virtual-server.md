---
author: slowe
categories: Rant
comments: true
date: 2007-06-15T09:03:49Z
slug: leaving-esx-for-virtual-server
tags:
- ESX
- Microsoft
- Virtualization
- VMware
title: Leaving ESX for Virtual Server?
url: /2007/06/15/leaving-esx-for-virtual-server/
wordpress_id: 472
---

According to [this article](http://servervirtualization.blogs.techtarget.com/2007/06/04/vmware-esx-configuration-cost-problems-spur-users-switch-to-microsoft-virtual-server/), cost and configuration issues are spurring at least one organization to ditch [VMware ESX Server](http://www.vmware.com/products/vi/esx/) for [Microsoft Virtual Server](http://www.microsoft.com/windowsserversystem/virtualserver/). Here's a quote from the article:

>Today, Quigley, a senior network engineer, told me that his firm, Total Quality Logistics, LLC, will be migrating over to Microsoft Virtual Server 2005 R2, over the next 45 days. TQL  up to now a VMware shop  will only use ESX in the lab. ESX is too expensive to upgrade and requires more training and resources than TQL can deliver, Quigley said.

So, let me see if I have this straight. Virtual Server, which is given away for free, is cheaper than ESX Server. Well, that seems fairly obvious---it's hard to get any cheaper than free, isn't it? So is it really possible to make a viable comparison between two products based on price when one product (Virtual Server) is given away for free because Microsoft makes all its money elsewhere (Windows), but the second product (ESX Server) costs money because it is one of VMware's core products? Or do you have to consider other factors as well?

When it comes to comparing Virtual Server and ESX Server, Microsoft wants you to consider the price of Virtual Server (free) against ESX Server (not free). But when it comes to comparing Linux (free) and Windows (not free), Microsoft's on the other end of the stick, and they want you to consider other factors---total cost of ownership, training, etc. So do we consider factors other than just cost, or not?

I think most everyone would agree that with regards to features, ESX Server (as part of a Virtual Infrastructure 3 implementation) clearly outshines Virtual Server. After all, Virtual Server doesn't have VMotion (live migration), DRS, HA, resource pools, or any such features. With regards to cost, Virtual Server is as inexpensive as it comes. ESX Server wins on features, Virtual Server wins on cost.

In this case, we now need to consider the cost of administering or managing your virtualization solution. Some would say that Virtual Server is easier to manage than ESX Server. I guess that depends greatly upon who is managing the environment. I know of a VMware administrator who single-handedly manages 22 ESX Servers hosting more than 150 virtual machines. Would this administrator consider Virtual Server "easier to manage" than ESX Server? Probably not. On the other hand, a Windows system admin whose grown his/her career managing Windows boxes won't find ESX Server as easy to manage as a Windows server. It's really all a matter of experience and perspective.

So rather than taking this story and trying to tear it down, let's recognize it for what it is. One organization evaluated Virtual Server and ESX Server based on the criteria important to that organization (cost and management), and felt that Virtual Server was the _right_ choice for them. It won't be the right choice for every organization, just as ESX Server isn't the right choice for all organizations. And it's _not_ validation that Virtual Server is _better_ than ESX Server, as the definition of "better" will change from organization to organization. For many organizations, the cost of VMware licenses are outweighed by the savings introduced by higher consolidation ratios and the enterprise-class feature set.

Personally, I think that most, if not all, of the issues that were described in that article could have been resolved with a correct configuration of ESX Server, but that's just my personal opinion.
