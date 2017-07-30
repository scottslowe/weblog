---
author: slowe
categories: Information
comments: true
date: 2008-09-09T09:50:42Z
slug: virtualization-short-take-18
tags:
- Citrix
- ESX
- HP
- HyperV
- Microsoft
- VDI
- Virtualization
- VMware
- VMwareHA
- VMworld2008
title: 'Virtualization Short Take #18'
url: /2008/09/09/virtualization-short-take-18/
wordpress_id: 878
---

Only a few little "bits and bytes" in this week's Virtualization Short Take, as most of the activity seems to be involved in making product announcements. Product announcements are quite well handled on other sites, although I include them here from time to time. In any case, here a few links I found over the last week or so. I hope you find them interesting and useful.

* Alessandro [reported](http://www.virtualization.info/2008/09/microsoft-offers-api-for-3rd-vdi.html) that Microsoft has an API for third-party connection brokers to leverage. More details and the original post are [here](http://blogs.msdn.com/ts/archive/2008/08/12/how-to-extend-the-ts-session-broker-to-support-vdi-part-1.aspx). This just goes to underscore my belief that desktop virtualization will be the real battlefront between the major virtualization vendors, and this is an area where VMware has the least longevity. Consider the players: Microsoft, who makes the desktop operating systems being virtualized, and understands them better than anyone else; Citrix, who's been delivering applications and desktops for years and years; and VMware, who has only really moved into the desktop space in the last couple of years. VMware does have some strengths in this area, no doubt, but their competitors are quite formidable.

* A recent [performance comparison between VMware ESX and Microsoft Hyper-V](http://www.networkworld.com/reviews/2008/090108-test-virtualization.html) revealed some very interesting information. Network World, who conducted the comparison, ends up giving the nod to VMware ESX overall, but I was mildly surprised at how well Hyper-V did (I expected the numbers to be worse for Microsoft).

* The market and the competition around display protocols really seems to be heating up. [Via Alessandro](http://www.virtualization.info/2008/09/vdiworks-connection-broker-to-support.html), I learned that VDIworks will be supporting HP's Remote Graphics Software (RGS), a high-performance remote display protocol that HP leverages in their workstation blade solutions. Add to that the work that VMware is doing on Net2Display, Qumranet's (now Red Hat's) work on SPICE, updates to RDP from Microsoft, and Citrix's ICA protocol, and now suddenly you have quite a crowded remote display protocol market. If I were a betting man (which I'm not), I might be inclined to start a pool to see who wins.

* Here's something that may have gone unnoticed by many: VMware has released [VMware Studio](http://www.vmware.com/download/sdk/studio.html), a tool for allowing developers or ISVs to create and distribute their own virtual appliances in OVF (Open Virtualization Format). Thanks to [Greg Lato](http://www.latogalabs.com/2008/09/vmware-studio-released/) for the news.

* Duncan is on a bit of a VMware HA theme recently, with a couple of posts pointing out [how to change the isolation response timeout](http://www.yellow-bricks.com/2008/09/08/ha-isolation-response-shutdown-guest/) and an [update on the location of FT_HOSTS](http://www.yellow-bricks.com/2008/09/08/ft_hosts-where-is-it-in-esx-35-u2/), an important portion of VMware HA's operation. Be sure to have a look at these two articles.

That's it for now. Keep in mind I'll be liveblogging as much of VMworld 2008 as possible, and I'm considering adding Twitter to the mix as well; I haven't decided. Let me know what additional forms of information you may find useful, if any, and I'll see what I can add.
