---
author: slowe
categories: Liveblog
comments: true
date: 2017-04-19T00:00:00Z
tags:
- Docker
- DockerCon2017
title: 'Liveblog: DockerCon 2017 Day 2 Keynote'
url: /2017/04/19/dockercon-2017-day-2-keynote/
---

This is a liveblog of the day 2 keynote (general session) of DockerCon 2017 in Austin, TX. For a look at what was announced or discussed in the day 1 keynote yesterday, see [this liveblog][xref-1]. You can also see all DockerCon 2017-related posts by browsing the posts tagged with "DockerCon2017" (see the links at the bottom of this page). Before the keynote starts, there's some nice live music playing; a welcome change (in my opinion) from yesterday's video game.<!--more-->

At 9:03am, Ben Golub takes the stage to kick off the day 2 general session. He starts off by reviewing some proposed Docker logos, with a hint toward an announcement at the end of the session (presumably around changing Docker's logo).

Golub then transitions into the meat of the general session presentation, which (understandably) is focused on Docker in the enterprise. He reviews the usual slide with notable logos from Docker customers. He also discusses some results from a company called ETR, which (apparently) shows Docker is "off the charts" in terms of adoption and market penetration within the enterprise. Golub also debunks the bi-modal IT structure model, saying that Docker's customers only want one thing: speed (as in moving faster, not as in performance).

In talking about how some customers are approaching Docker from a microservices-first architecture and how other customers are approaching Docker from the perspective of Docker-izing traditional applications, Golub makes the claim that simply Docker-izing a traditional application---without changing a line of code or changing any of your infrastructure---customers can see a 4x-5x improvement in infrastructure efficiency. (This is quite a bold claim, and I'd love to see the data behind it.)

Golub continues the discussion on how Docker makes you not only future-proof (enabling you to adopt future technologies more easily) but also past-proof (protecting investments in older technologies). In order to make this a reality, though, Docker needs to be diverse---it needs to support lots of different platforms and lots of different environments.

At this point, Golub brings up Swamy Kocherlakota from Visa up to the stage to talk. Kocherlakota takes a few minutes first to talk about Visa before shifting gears to discuss Visa's technical approach. I'm not sure I can agree with Kocherlakota's implied association between virtualization and some of their technical headaches (painful patching, for example---how is that a virtualization problem?), but he quickly moves into a discussion of how virtualization is inefficient and causes long infrastructure provisioning times. Once again, I'm not clear how using Docker fixes infrastructure provisioning delays, or how using Docker fixes issues with patching and hardware maintenance. However, Kocherlakota's focus on developer productivity and a standard way of packaging and deploying apps seems like a "home run" for Docker and Docker's ecosystem, so that makes perfect sense. Ultimately, Visa is trying to move all developers and application groups to a "container-first" microservices-based architecture.

Golub re-takes the stage to reinforce some "lessons learned":

1. Start with a secure base (that embraces diversity)
2. Secure the whole supply chain (and keep that diversity)
3. Leverage an ecosystem (that supports diversity)

Golub's focus on diversity leads into a discussion of Docker Editions (Docker for Mac, Docker for Windows, Docker for AWS, etc.), and then into a discussion of CaaS (Containers-as-a-Service). After a brief mention of the need for security in these editions, Golub moves on to discuss how to apply security to the supply chain; specifically, how to get stuff from developers to the image registry/CaaS. This concludes in a review of image scanning, secure access and user management, policy management, content verification, etc.

At this point, Golub brings out a demo team (Lily and Vivek from last year's demo) to show off a secure supply chain. The demo shows off Docker Trusted Registry (DTR), Docker Security Scanning, and Docker Compose.

After the demo time steps off the stage, Golub reviews the demo and reviews the announcement regarding Docker Store and certified third-party container images. This, in turn, leads to a review of Docker Enterprise Edition, which includes Docker for AWS, Docker for Azure, etc. Major expansions to the list of Docker Editions this week include Docker running on z Systems and Power Series, and Docker for GCP. Docker also is partnering with Alibaba Cloud to provide Docker in Alibaba's public cloud as well as in Alibaba's private cloud offering (Aspara Stack). More than 60% of the 20 billion dollars in transactions that occur on Singles Day (November 11) happens via Alibaba, and Alibaba's platform is built on Docker.

Golub shifts gears slightly to discuss a braoder ecosystem of delivery, mentioning the global systems integrators and Federal systems integrators.

Lily and Vivek are now brought back to the stage to demo deploying certified third-party applications via Docker. The demo shows off using an open source tool called `image2docker` that takes a VMDK and converts it into a `Dockerfile` that can then be used to build a Docker image. The demo also showed off third-party applications (Oracle, specifically) available via the Docker Store.

At this point, Golub invites Mark Cavage from Oracle to take the stage and talk a bit more about Oracle's decision to make a number of Oracle software products available via the Docker Store. Products from Oracle available via Docker Store include Oracle Database, Oracle WebLogic, Oracle Coherence, Oracle Linux, and Java.

Golub comes back up to the stage, and talks about an effort to modernize traditional applications that is a turnkey program that includes consulting and software. The program comes from key partners like Avanade, Microsoft, Cisco, and HPE.

Before the keynote wraps up, Golub indicates there's one more use case, one more customers, that is going to describe their experience with Docker. Golub brings up Aaron Ades of MetLife.

Ades spends a few minutes talking about MetLife (which is celebrating 150 years next April), notable points in MetLife's history (they have 35-year-old code still running today!), and the breadth of MetLife's systems of record. All in all, Ades' presentation---while still short on technical details---is actually quite enjoyable. I particularly enjoyed the "antibodies to the status quo" quote from Ades' presentation---this something _every_ person and organization needs.

Golub comes back to the stage to close up the keynote. He reviews some of the sessions that will revisit some of the topics covered in the general session, and points out some of the customer use case sessions that will take place later in the day as well. Notably, Golub does _not_ say anything about the potential logo change to which he alluded at the start of the keynote.



[xref-1]: {{< relref "2017-04-18-dockercon-2017-day-1-keynote.md" >}}
