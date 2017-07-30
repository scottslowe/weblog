---
author: slowe
categories: Information
comments: true
date: 2015-06-24T09:15:00Z
tags:
- Docker
- DockerCon2015
- Automation
- Storage
- Security
title: DockerCon Vendor Briefings
url: /2015/06/24/dockercon-vendor-briefings/
---

At DockerCon 2015 in San Francisco, I had the opportunity to meet with a few vendors in the Docker ecosystem. Here are some notes from my vendor briefings.

## StackEngine

[StackEngine][link-1] describes themselves as enterprise-grade container application management. They tout features like being able to compose Docker applications using a drag-and-drop interface, deploy containers across multiple hosts, and provide automation---all with the sort of controls that enterprise IT groups are seeking. That's all well and good, but the key problem in my mind is that these are features Docker is seeking for themselves. Docker Compose offers the ability to specify applications. True, there's no GUI (yet). Alas, StackEngine can translate their GUI application design into YAML, but it doesn't comply with Docker Compose. Thus, it ends up being more competitive than complimentary, in my opinion. Docker Swarm and the upcoming Docker Network address some of StackEngine's deployment functionality, and if Project Orca takes off as an official effort---well, let's just say I hope that StackEngine has more planned. This is not to say that StackEngine isn't a well-engineered solution offering real value; rather, this is to say that StackEngine appears to be, unfortunately, in the crosshairs for functionality Docker is aiming to provide themselves. VMware was criticized in years past for "eating the ecosystem"; it looks like we'll see some of the same here with Docker.

## Sumo Logic

[Sumo Logic][link-2] is one of two SaaS offerings in the Docker ecosystem with whom I spoke this week at the conference. Both companies tout "analytics" and "machine learning" as key values of their offering. Sumo Logic strikes me as a SaaS version of Splunk or VMware Log Insight, perhaps with some additional analytics functionality and some extra integration with various SaaS platforms (like Salesforce) and IaaS platforms (like AWS). Their integration with these other SaaS and IaaS offerings gives them a bit of an advantage over other, similar competitors that can't offer this level of visibility into those services and platforms. If a deeper level of integration into these offerings and platforms is important to your business, then Sumo Logic may have an edge for you.

## SignalFx

[SignalFx][link-3] is the second of the two analytics-focused SaaS offerings I encountered this week. Unlike Sumo Logic, SignalFx's key differentiator is real-time, in-stream data analytics, especially for metrics coming from instrumented, cloud-native applications. Log data is certainly important, but aggregating logs and letting users slice/dice/query that data isn't what SignalFx is really trying to do. Instead, they're focusing on companies building next-gen applications, providing a SaaS offering that will enable developers at these companies to build instrumentation into their next-gen apps and have the metrics from that instrumentation provide greater visibility into behaviors, trends, issues, etc. In other words, the real power of SignalFx comes when an organization wants to gain deeper real-time visibility into their applications *and* are willing to have their developers add instrumentation to make that happen. Thus, the "sweet spot" of the market for SignalFx are the companies where that can happen. If you're a shop with lots of COTS software, this isn't for you. However, if you're a shop where developers consume resources from a private cloud and need to support the apps you're writing and deploying, this might be an option to explore.

## ClusterHQ

[ClusterHQ][link-4] is the company behind Flocker, which finally reached 1.0 status last week. ClusterHQ, along with a couple other companies, has been crucial in the formation of Docker Plugins. This makes sense, of course, because it benefits Flocker---which will be able to take advantage of the storage plugin endpoint to modify/customize/extend the behavior of Docker volumes. _(Side note: Docker Plugins are part of the experimental builds for now.)_ My primary concern about ClusterHQ (and I shared this concern with them) is that the introduction of the storage plugin endpoint now, in some ways, renders Flocker obsolete. Big name storage companies---EMC, NetApp, HDS, HP, IBM, etc.---as well as any number of smaller, more nimble companies---like Pure, SolidFire, Nimble, Tintri, etc.---can just write their own storage volume plugins for Docker and be done with it. Why bother with ClusterHQ and Flocker? ClusterHQ thinks that offering a "more stable API" along with the ability to potentially integrate with other platforms later (thus hinting that ClusterHQ has more up their sleeve) is compelling enough to avoid this scenario. Time will tell.

## Skyport Systems

I also had a very brief conversation with Doug Gourlay and Nils Swart from [Skyport Systems][link-5]. They are doing something _very_ interesting with regards to---to use their phrase---hyper-secured infrastructure. I won't go into great detail here, but I'm looking forward to talking with them again to gain more details and a better understanding of what their solution looks like. Stay tuned for more.



[link-1]: http://stackengine.com
[link-2]: https://www.sumologic.com
[link-3]: https://signalfx.com
[link-4]: https://clusterhq.com
[link-5]: https://www.skyportsystems.net
