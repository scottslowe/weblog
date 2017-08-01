---
author: slowe
categories: Rant
comments: true
date: 2009-04-03T17:02:24Z
slug: a-collection-of-viewpoints-on-cisco-ucs
tags:
- Cisco
- FCoE
- Hardware
- HP
- IBM
- Networking
- Sun
- UCS
- Virtualization
title: A Collection of Viewpoints on Cisco UCS
url: /2009/04/03/a-collection-of-viewpoints-on-cisco-ucs/
wordpress_id: 1266
---

A couple of weeks have passed since the announcement of Cisco's Unified Computing System (UCS), and in that timeframe I've collected a number of links to articles and blog posts about UCS. I thought I'd collect them here and try to get a feel from all of the various viewpoints where the industry stands on UCS.

I'll start with Robin Harris aka StorageMojo and his [initial take on UCS](http://storagemojo.com/2009/03/16/ciscos-unified-computing-system/). The one thing that jumped out at me about his article was this statement:

>If IBM, HP and Sun aren't meeting today to plot a radical, Cisco margin destroying open-source router & low-cost switch counterattack - like Seagate, HP and IBM performed on Quantums DLT - they're idiots.

This seems to validate the [strategy outlined by Sun][1] and sheds new light---for me, at least---on the potential motivations for IBM to acquire Sun and, thus, Sun's intellectual property. Is Big Blue's move to acquire Sun a precursor to a strike against the heart of Cisco's routing and switching business? And how would Cisco respond to just such a move?

Massimo Re Ferre' of IT 2.0 [approaches UCS](http://it20.info/blogs/main/archive/2009/03/31/203.aspx) from a different angle. According to Massimo, if you stop and really look at what you get from UCS, it's not terribly different from what you can get from other vendors. In fact, if you separate out the unified fabric, there really isn't a whole lot to distinguish UCS from other, similar solutions from HP, IBM, Dell, and Sun. And if you think about it, he's right---it's really only the unified fabric, along with the fabric extenders in the chassis and the single point of management, that differentiate the platform.

Therein lies the problem. Massimo points out a couple of potential problems with unified fabric (security and political/organizational challenges). If unified fabric doesn't fly, then UCS is grounded too. And industry excitement over FCoE isn't exactly the greatest in the world. Chris Evans aka The Storage Architect makes clear his feelings about FCoE in [this post](http://thestoragearchitect.com/2009/03/31/enterprise-computing-the-inevitable-fcoe/):

>FCoE is a Cisco strategy to own the data centre, nothing else. As the recession bites, it would be a brave soul who could justify the disruption and additional spend, for very little gain.

FCoE is hardly a forgone conclusion, and given that so much of UCS' value is tied up in the unified fabric and the results that come from it, that makes UCS awfully vulnerable.

Not everyone thinks that FCoE makes the UCS vulnerable, by the way; technology evangelist Christopher Kusek thinks [the future will be a unified one](http://www.pkguild.com/2009/03/16/the-future-will-be-a-unified-one-and-cisco-will-be-there/):

>The Data center has spoken and its answer is **True unification.**

Burton Group analyst [Drue Reeves says](http://dcsblog.burtongroup.com/data_center_strategies/2009/03/cisco-unveils-unified-computing-system.html) that this was a move Cisco _had_ to make:

>In the end, UCS was a move Cisco had to make to ward off competition AND increase shareholder value. Cisco has a strong brand, enterprise credibility, the technical chops and finances to pull it off. Is UCS a business risk? Sure. But the greater risk for Cisco is to do nothing.

Perhaps, perhaps not. Given that UCS relies so heavily on FCoE, wouldn't it have made sense for Cisco to push the FCoE train along by providing FCoE interconnects for blade servers from HP and IBM? Of course, this is supposing that CNAs would be available for HP and IBM blades, but I'm sure that this is something Cisco could have helped guarantee. This route would have broadened the market for FCoE and the unified fabric and simultaneously establishing Cisco as _the_ FCoE leader (as if they weren't already). Then, when really game-changing stuff like the [SR-IOV-enabled adapters like "Palo"][2] were available, Cisco could have taken the leap into the compute space and played the unified management card. That seems like a less risky approach to me. But hey, what do I know?

[1]: {{< relref "2009-03-12-this-pretty-much-answers-that-question.md" >}}
[2]: {{< relref "2009-03-18-cisco-ucs-virtualization-optimized-cnas.md" >}}
