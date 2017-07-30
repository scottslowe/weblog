---
author: slowe
categories: Liveblog
comments: true
date: 2015-04-30T12:28:00Z
tags:
- Cloud
- Docker
- Interop2015
title: 'Interop Liveblog: The Post-Cloud'
url: /2015/04/30/the-post-cloud/
---

This session is titled "The Post-Cloud," and the speaker is Nick Weaver, Director of SDI-X at Intel.

Nick starts his presentation with a summary of our society: some people produce goods through an effort, and others consume what is produced. Things have changed over the years that have affected this production-consumption model, but Nick quickly turns his focus to the use of machines in the production portion of this cycle. As production efficiency increased, the level of consumption also increased. This is especially true for computing machines, and how people consume the services/information produced by the computing machines.

This brings Nick around to a discussion of Jevons' Paradox, which basically states that the increased efficiency of producing something actually leads to an increase in consumption, not a decrease of consumption.

So what does efficiency in technology look like? Technology enables things; by itself, it doesn't really add value. Therefore, efficiency in technology means enabling more (or more powerful) things. Nick starts his discussion on technology efficiency with a discussion of DevOps, and what DevOps means. Although a number of technologies are involved to deal with the ever-increasing complexity and density that has emerged, DevOps is really about a culture change. DevOps favors automation over repetition, decoupling deployment from operations---it means making technology practitioners more effective and more efficient (thus enabling technology to enable more).

This leads to a discussion of CloudFoundry and Platform-as-a-Service (PaaS). PaaS helps manage all the various "things" that are necessary to deploy an application, thus again making it easier to consume applications (and therefore enabling applications/technology to enable more). PaaS can help with microservice architectures, because PaaS often provides things like service discovery and other "glue" necessary to stitch all the various pieces together. PaaS increases efficiency.

Now the discussion shifts to Docker containers. Nick takes a moment to explain some of the underpinnings (cgroups and namespaces) that make Docker containers possible, drawing an analogy between cgroups/namespaces with VMware resource pools in vCenter. He draws some comparisons between the use of full-machine virtualization (aka VMs) with the use of Linux containers---not that one is better or worse than the other, but that they are different and therefore have different characteristics and different use cases.

After talking about containers internals and some of the (potential) benefits of containers, Nick shifts to a focus on Docker specifically. Docker's value is in streamlining the use of containers, i.e., making the experience super-easy. This includes the use of layers, and making these layers easy to share and recombine to create other things. Docker increases efficiency by reducing the cost of using containers both in operational overhead (time) and in needing particular expertise. Docker increases efficiency because it's easy to use.

Next up in Nick's discussion is big data. The first problem is efficient access to data. The second problem is extracting useful information for a particular domain (and each domain is particular to a person, industry, task, etc.). Lots of tools here---Hadoop, Spark, Cloudera, Storm, etc. These tools provide efficiency in access, storing, and extracting information, which in turn enables efficient decision making.

Now we're talking about Google. Nick mentions that Google makes extensive use of containers, to the order of creating and destroying 2 billion containers a week. (This number has been tossed around a few times.) This extensive use of containers has lead to the rise of scheduling/orchestration tools like Kubernetes, Mesos, and Docker Swarm. Naturally, the scheduling of containers at Google's scale can't be done manually, and has to be done intelligently. So how do these tools increase efficiency? They increase efficiency by making more effective use of utilizing resources to run applications/workloads/services.

Nick begins to wrap things up with "The Road to Awe." If the problem was "I want to deploy my web service", the initial answer was the x86 server (which made computing power more accessible to people). The next answer was to use multiple virtual servers, for more density. Next we wanted workload mobility (live migration), and then deploying web services as a service (IaaS). That lead to "automagically configured" web services (PaaS). Then we wanted captured and immutable images of our web services (Docker), being able to turn them up extremely quickly (Linux containers and Docker) and easily integrated into our Continuous Development lifecycle. Finally, we wanted to be able to do all that on-demand, and quickly replaced on error (Mesos, CF Diego, Kubernetes). That, in turn, leads to wanting to be able to manage and place the workload intelligently based on data from any level---i.e., i just want to run a web service and have the data center do all the rest (the Post-Cloud).

It's about pushing for the (Jevons') Paradox----making things easier to produce so that others can consume what we are producing.

Nick breaks things down into Watcher (it's watching and collecting data from all sorts), feeding that data into the Decider (which makes decisions). The decision is fed to the Actor, which acts upon decisions made by the Decider based on data collected by the Watcher. Finally, the entire cycle is "wrapped" together by a learning cycle that helps the Watcher-Decider-Actor cycle work better than it did before (i.e., integrate machine learning).

So...what has to come next? What is needed to make this happen?

* Greater exposure of telemetry (more information from more sources, greater access to the state of things)
* Google Omega-style decision making for the masses (fed by knowledge built on telemetry)
* Brand new disciplines around extracting efficiency
* Massive cooperation of cloud domains

What is "the Post-Cloud"? In conclusion, Nick defines the Post-Cloud as what emerges from the next evolution of efficiency in computing. It is also just a label for the computing that our grandchildren will take for granted as they provide their own value.

With that, Nick opens the floor to questions and wraps up the session.
