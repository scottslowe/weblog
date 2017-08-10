---
author: slowe
categories: Musing
comments: true
date: 2010-09-20T20:56:53Z
aliases: /2010/09/20/thinking-out-loud-ospf-summary-routes/
slug: trying-to-understand-ospf-summary-routes
tags:
- Cisco
- Networking
title: Trying to Understand OSPF Summary Routes
url: /2010/09/20/trying-to-understand-ospf-summary-routes/
wordpress_id: 2107
---

While working through some OSPF configurations in preparation for my CCNA exam next week, I noticed something I thought was odd, and I don't really understand why it's behaving in this manner. I thought perhaps a networking expert can enlighten me.

First, a quick summary of what I have configured:

* I have an OSPF area 0 with a single backbone router contained entirely in area 0 and two area border routers (ABRs). The ABRs span area 0 and areas 1 and 2: one router is connected to both area 0 and area 1, the other is connected to both area 0 and area 2.

* I have an `area _X_ range` command in the configuration of the routers. For example, area 0 contains only networks in the 192.168.0.0/19 range, so I used `area 0 range 192.168.0.0 255.255.224.0` in the configuration of the area 0 router.

* Similarly, the ABRs for areas 1 and 2 also contain `area _X_ range` statements. Area 1 uses 192.168.32.0/19; area 2 uses 192.168.64.0/19. The `area _X_ range` statements are configured appropriately for each ABR and each area.

The behavior I'm seeing is that the route summarization works; instead of showing different routes for all the subnetworks in area 1, the routers in areas 0 and 2 only show the summary route. That's all as expected. Also expected is that the ABRs show the detailed routes within the areas to which they are connected.

What's not expected, though, is that the ABRs also have a summary route for their own area that is connected to the Null0 interface. For example, the ABR that connects to both area 0 and area 1 shows detailed routes for areas 0 and 1, a summary route for area 2, and another summary route for area 1 connected to Null0.

The same is true for both ABRs, but not for the backbone router whose interfaces are contained entirely in area 0.

So what's up with this Null0 interface and associated route?
