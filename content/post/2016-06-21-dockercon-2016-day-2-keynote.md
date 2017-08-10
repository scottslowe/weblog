---
author: slowe
categories: Liveblog
comments: true
date: 2016-06-21T00:00:00Z
aliases: /2016/06/20/dockercon-2016-day-2-keynote/
tags:
- Docker
- OSS
- Linux
- DockerCon2016
title: DockerCon 2016 Day 2 Keynote
url: /2016/06/21/dockercon-2016-day-2-keynote/
---

This is a liveblog for the day 2 keynote of DockerCon 2016, which wraps up today in Seattle, WA. While today's pre-keynote warm-up doesn't include laser-equipped kittens, the music is much more upbeat and energetic (as opposed to yesterday's more somber, dramatic music). If the number of laptops on the podium is any indicator (yesterday it was a cue to the number of demos planned), then today's keynote will include a few demos as well.

Ben Golub kicks off the day 2 keynote---with the requisite coffee shot that is a sacrifice to the "demo gods"---and offers up some thanks to the supporters of last night's party at the Space Needle. Golub quick reviews the key announcements and demos from the day 1 keynote (see my liveblog [here][xref-1]). Today, though, will be focused on democratizing Docker in the enterprise. In referring to Docker's adoption in the enterprise, Golub shares some numbers that vary widely, and admits that it's really difficult to know what the real adoption rate is. He points to multiple "critical transformations" occurring within the enterprise: application modernization, cloud adoption, and DevOps (process/procedure/culture changes).

This leads Golub into a discussion of anti-patterns, or fallacies. The first fallacy he tackles is the idea of "bi-modal" IT (which I'm sure won't make him popular with Gartner). The second anti-pattern (or fallacy) is the PaaS fallacy, which says that the solution limits you in the languages, frameworks, and infrastructure you can use. (This won't make the CloudFoundry people happy.) Slightly related to this anti-pattern is the third one Golub tackles, the private IaaS fallacy; Golub doesn't believe that it's necessary to build a private cloud in order to become agile. (I suppose this greatly depends on your definition of "agile"; a very limited definition may support some of these viewpoints.)

The key, according to Golub, is incremental revolution (not requiring radical changes). I would posit that none of the three anti-patterns Golub discussed necessarily _require_ radical change and could be implemented using incremental revolution, but Golub believes that Docker is well-positioned to offer customers incremental revolution and agility across multiple use cases and for multiple application types (including legacy applications).

At the same time, Golub reinforces the need not only for agility and portability, but also for control (particularly in the enterprise, according to Golub). Golub believes this ends up looking a lot like the modern physical supply chain. This leads Golub into a discussion of containers-as-a-service (CaaS), which is _not_ a service made up of containers but a service that provides containers. This is the right level; not too high (PaaS) nor too low (IaaS). Further, it offers the right set of controls and governance.

Naturally, Docker has a product that enables CaaS; this is Docker Datacenter (comprising Docker Trusted Registry, Docker Universal Control Plane, integrated security, and the Docker Engine itself). Golub paints Docker Datacenter as an open, non-opinionated solution.

To demo Docker Datacenter, Golub calls up Lily Guo and Vivek Saraswat. Guo and Saraswat reuse the "ops-versus-dev" interplay that worked so well yesterday (with the addition of "the business guy"). The demo includes deploying an application from the experimental Distributed Application Bundle (DAB) format, scaling the application, using role-based access control (RBAC), and security scanning features. Much of the functionality of the demo was already shown yesterday; what was not shown yesterday was the Trusted Registry security scanning and the RBAC functionality of Docker UCP.

Next up, Golub once again reviews the tremendous growth in Docker images and Dockerized applications, and this leads to an announcement of the Docker Store (the "App Store for Docker"). The Docker Store supports both free and paid content, and has mechanisms in place for payment, entitlements, etc.

Golub shifts his discussion to the Docker partner ecosystem, reviewing a few different statistics shared in yesterday's keynote as well. Adopting Docker in the enterprise "takes an ecosystem," according to Golub, and he reviews the various parts of the ecosystem that Docker (and Docker partners) offer. This includes things like 3rd party software, tools/plugins/integrations, support/services/training, and integration/procurement. On the topic of infrastructure---where Golub reminds everyone that Docker runs on any infrastructure---Golub specifically calls out the recent announcement with Hewlett Packard Enterprise (HPE). HPE will be bundling Docker with every ProLiant server (though I still haven't seen anything around the Linux distribution that will be used). Golub also calls out Microsoft, mentioning not only the work being done to run Docker on Windows but also the collaboration with Microsoft around Azure.

To talk more about the partnership with Microsoft, Golub invites Mark Russinovich, CTO for Microsoft Azure, on stage. Russinovich shows that Docker Datacenter available in the Azure Marketplace, and then proceeds to demo a running Docker Datacenter implementation running in Azure. Russinovich's demo looks a bit like yesterday's demo in that he's using Visual Studio Code on OS X with Docker for Mac to do live debugging; the difference is that he's debugging C# code. Russinovich also incorporates Azure Stack into his demo to show that his Docker Datacenter cluster spans both public Azure resources and on-premises resources made available via Azure Stack. Although part of Russinovich's demo fails (the danger of live demos!), he handles it gracefully and recovers quickly. Russinovich's demo also incorporates the use of Microsoft SQL Server running on Ubuntu. With that, he "drops the mic" and leaves the stage.

Golub returns to the stage and resumes his "it takes an ecosystem" theme, this time focusing on user stories. To tell such a user story, Golub invites Keith Fulton, CTO of ADP, to take the stage. Fulton reviews ADP and why ADP feels Docker is critical to ADP's success. It all boils down to speed---being able to build applications, deploy applications, and troubleshoot applications faster. Fulton believes Docker shows a new path to "DevOps", providing a clear separation between packing the containers (dev) and shipping the containers (ops). The common link is, naturally, the container. Fulton highlights security, scale, and legacy monolithic apps as unique differentiators of ADP's use of Docker containers. (Fulton does use a very entertaining comparison of microservices and monoliths to chicken nuggets and chickens in his talk.)

At this point Golub takes the stage again to close out the keynote.


[xref-1]: {{< relref "2016-06-20-dockercon-2016-day-1-keynote.md" >}}