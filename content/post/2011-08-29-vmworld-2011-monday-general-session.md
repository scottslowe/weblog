---
author: slowe
categories: Liveblog
comments: true
date: 2011-08-29T18:45:19Z
slug: vmworld-2011-monday-general-session
tags:
- vCloud
- Virtualization
- VMware
- VMworld2011
- vSphere
title: VMworld 2011 Monday General Session
url: /2011/08/29/vmworld-2011-monday-general-session/
wordpress_id: 2383
---

This is the session blog for the Monday general session. I'm fortunate enough to have arrived in time to get a seat at the blogger/press/analyst tables. While the network connectivity is good, the power is---unfortunately---not so good.

The general session started with an impressive lightshow across the front of the conference that depicts the change of computing with the advent of virtualization and cloud computing. It was visually appealing and interesting.

At the conclusion of the visual show, Rick Jackson, Chief Marketing Officer for VMware, takes the stage to kick off the general session. Rick indicates that there are about 19,000 people here at VMworld 2011 this week; attendance is down, understandably, due to Hurricane Irene's effect on the East Coast of the United States and the resulting impact on air travel.

Rick indicates that the Hands-On Labs for VMworld 2011 are completely hosted on public clouds: Switch SuperNAP, Colt, and Terremark all provide public cloud services for this year's labs. The labs are built on vSphere 5.0 and vCloud Director 1.5. Both Paul Maritz and Carl Eschenbach will be speaking later in this session; and tomorrow morning VMware CTO Steve Herrod will be doing a technology keynote to demonstrate what VMware's working on.

Rick also confirms that VMworld 2012 will be back in San Francisco (yay!), being held from August 27 to 30, 2012. At this point, Rick introduces Paul Maritz, CEO of VMware, and gives him the stage.

Paul gives some statistics:

* One VM being deployed every six seconds (that's faster than babies being born in the US)

* 20 million VMs running on VMware vSphere

* More VMs in flight using vMotion than there are aircraft in flight

* Greater than 800,000 vSphere administrators (that's the population of San Francisco)

* Greater than 68,000 VMware Certified Professionals (across 146 countries)

* More than 1,650 ISV partners and more than 3,000 apps certified on VMware

So, given all this success, where does VMware go from here? This sets Paul up to give VMware's vision and explain the various forces that are at work in the transformation of IT in this "unfolding cloud era." Paul takes us on a journey from his early days in IT and how the industry transformed during the client-server era and now into the cloud era. For the most part, this is the same material that we've seen in previous conferences, but with one notable addition: a strange focus on data fabrics (the relational database, for example). Maritz says that the relational database as a data fabric simply cannot handle the scale of traffic that the cloud era demands.

Maritz spends some time talking about the tasks that need to be completed to help us move into the cloud era, and ties that to vSphere versions that have been delivered by VMware in recent years (4.0 in 2009, 4.1 in 2010, 5.0 in 2011). The delivery of vSphere 5 is a key part of the first task to be completed: modernize infrastructure and operations.

VMware is also aggressively target public cloud-based services running on vCloud Director, and Maritz announces a couple new vCloud partners. Not leaving out the sizable SMB market, Paul Maritz also described VMware's commitment to that marked with a new release of vSphere Essentials, and he touches base on VMware Go, a SaaS-based service to assist in getting their infrastructure setup and running.

The second task we must address to move into the cloud era is to handle the migration or transition of existing apps to new and renewed apps. This is the core of VMware's vFabric push: to build new frameworks, provide new platforms, and supply new data fabrics that are capable of handling the scale and volume that the new cloud era needs. SQLFire takes the extraordinary scalability of GemFire and enables people to use it with the more traditional SQL query language. VMware is also announcing vFabric Data Director, a new way of automatically provisioning and managing databases on vSphere. The first "example" or "implementation" of vFabric Data Director is vFabric Postgres, a vSphere- and vFabric-optimized version of Postgres to be used with vFabric Data Director and vSphere. The third aspect of vFabric and VMware's push to modernize applications is CloudFoundry, a new Platform-as-a-Service (PaaS) offering. CloudFoundry supports node.js, Ruby, and Spring. Scala support has been added by the open source community. To help with adoption, VMware has created a local version of CloudFoundry that can run on a local laptop.

The third task to move into the cloud era is addressing end-user access. To that end, VMware is announcing VMware View 5.0, with improvements in bandwidth usage, greater availability of View clients (clients for just about any device), and greater integration with VoIP/unified communications providers and services. View is, of course, only part of the strategy; there's also Horizon, VMware's offering to manage users and applications across traditional applications and "cloud era" applications. Horizon is no longer a single product, but a collection of products that allow IT to associate information and applications to people instead of devices. Maritz also makes references to MVP, VMware's Mobile Virtualization Platform. Virtual phones? We shall see.

At this point, Carl Eschenbach is brought onto the stage to transition into a discussion about moving to the cloud era from the perspective of three different customers who have made this journey themselves.

My battery is now running down, so I'm wrapping up this session blog.
