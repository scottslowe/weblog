---
author: slowe
categories: Information
comments: true
date: 2012-09-12T20:50:49Z
slug: idf-2012-day-1-summary-and-thoughts
tags:
- Hardware
- IDF2012
- Intel
- Networking
title: IDF 2012 Day 1 Summary and Thoughts
url: /2012/09/12/idf-2012-day-1-summary-and-thoughts/
wordpress_id: 2850
---

I just completed day 1 of Intel Developer Forum (IDF) 2012 in San Francisco. I tried to blog about as much as I possibly could. Here are the links to what I was able to capture during the day:

[IDF 2012 Day 1 Keynote][1]  

[Next-Generation Microarchitecture, code-named "Haswell"][2]  

[Data Plane Virtualization][3]  

[ODCA and Cloud Usage Models][4]  

One thing that really stuck out to me was an announcement made during a data center-focused press briefing directly after lunch. While the announcements made during the keynote were nice, they were consumer-oriented. The announcements during the press briefing, on the other hand, were much more enterprise data center-focused. The one thing that really stuck out to me was Intel's announcement of their Seacliff Trail reference platform.

The Seacliff Trail reference platform is a 1U top of rack (ToR) switch sporting 48 10 Gigabit Ethernet (GbE) ports and four 40 GbE ports. The platform supports OpenFlow (has been optimized for OpenFlow, in fact), and has hardware support for overlay encapsulations like VXLAN and NVGRE. Advanced networking technologies like TRILL, Shortest Path Bridging (SPB), Edge Virtual Bridging (EVB), and FCoE are also supported. Switching latency for cut-through switching is about 400 ns. Essentially, this is a reference platform for a pretty full-featured L2/L3 10 GbE/40 GbE ToR switch that can compete reasonably well with the "Tier 1" networking vendors like Cisco, Juniper, Arista, and others---presumably at a far lower cost.

Why did this stick out to me? To me, the introduction of mass-produced merchant silicon and an Intel reference platform for this sort of ToR switch sounds the death knell for networking vendors who differentiate themselves through hardware. It's the same thing that happened in the server hardware space. In the grander scheme of things, a Cisco UCS server is by and large the same as an HP ProLiant server and a Dell PowerEdge server. (Sorry, guys.) Sure, there are minor tweaks here and there from each of the major vendors, but these are mostly inconsequential. Now Intel is preparing to do the same to the 1U ToR network switch space, and it creates a lot of questions in my mind:

* What does this mean for the hardware-differentiated network vendors of the world? How do they continue to compete in this space? Does all of the innovation shift to software? If so, who among the "top tier" vendors is best poised to take advantage of this shift in development priorities?

* This switch has built-in support for OpenFlow. What does this mean for the adoption of the OpenFlow protocol? Who will emerge as the dominant supplier of OpenFlow controllers for all these OF-enabled ToR switches?

* This switch has hardware support for next-generation overlay protocols like VXLAN and NVGRE. What impact will that have on the uptake of these protocols in modern data centers?

Obviously, the answers to many (if not all) of these questions will be determined by the success (or failure) of the Seacliff Trail reference platform and the OEMs/ODMs that take up the reference platform. If Seacliff Trail becomes hugely successful, it could end up having quite an impact.

There are also other discussions that result from the Seacliff Trail announcement around convergence, but I'm going to hold on those discussions for the time being until I've had a bit more time to research and reflect. In the meantime, feel free to speak up in the comments below with your thoughts about what Seacliff Trail and Intel's move into the networking hardware space means to you. Please be sure to provide industry/employer affiliations where appropriate.

_(My disclosure: I work for EMC, but I'm attending IDF at the request of Intel. Intel is covering my expenses and provided a pass for the show.)_

[1]: {{< relref "2012-09-11-idf-2012-keynote-day-1.md" >}}
[2]: {{< relref "2012-09-11-spcs001-intel-next-generation-haswell-microarchitecture.md" >}}
[3]: {{< relref "2012-09-11-coms002-next-generation-cloud-infrastructure-with-data-plane-virtualization.md" >}}
[4]: {{< relref "2012-09-11-clds001-odca-and-usage-models-for-cloud-computing.md" >}}
