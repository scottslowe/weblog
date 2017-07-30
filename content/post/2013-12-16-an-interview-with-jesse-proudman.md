---
author: slowe
categories: Interview
comments: true
date: 2013-12-16T09:00:00Z
slug: an-interview-with-jesse-proudman
tags:
- Collaboration
- Cloud
- OpenStack
title: An Interview with Jesse Proudman
url: /2013/12/16/an-interview-with-jesse-proudman/
wordpress_id: 3384
---

I recently had the opportunity to conduct an e-mail interview with Jesse Proudman, founder and CEO of Blue Box. The interview is posted below. While it gets a bit biased toward Blue Box at times (he started the company, after all), there are some interesting points raised.

_**[Scott Lowe]** Tell the readers here a little bit about yourself and Blue Box._

**[Jesse Proudman]** My name is Jesse Proudman. I love the Internet's "plumbing". I started working in the infrastructure space in 1997 to capitalize on my "gear head fascination" with the real-time nature of server infrastructure. In 2003, I founded Blue Box from my college dorm room to be a managed hosting company focused on overcoming the complexities of highly customized open source infrastructure running high traffic web applications. Unlike many hosting and cloud startups that evolved to become focused solely on selling raw infrastructure, Blue Box subscribes to the belief that many businesses demand fully rounded solutions vs. raw infrastructure that they must assemble.

In 2007, Blue Box developed proprietary container-based cloud technology for both our public and private cloud offerings. Blue Box customers combine bare metal infrastructure with on-demand cloud containers for a hybrid deployment coupled with fully managed support including 24x7 monitoring. In Q3 of 2013, Blue Box launched OpenStack On-Demand, a hosted, single tenant private cloud offering. Capitalizing on our 10 years of infrastructure experience, this single-tenant hosted private cloud delivers on all six tenants today's IT teams require as they evolve their cloud strategy.

Outside of Blue Box, I have an amazing wife and daughter, and I have a son due in February. I am a fanatical sports car racer and also am actively involved in the Seattle entrepreneurial community, guiding the next generation of young entrepreneurs through the University of Puget Sound Business Leadership, 9Mile Labs and University of Washington's Entrepreneurial mentorship programs.

_**[SL]** Can you tell me a bit more about why you see the continuing OpenStack API debate to be irrelevant?_

**[JP]** First, I want to be specific that when I say irrelevant, I don't mean unhealthy. This debate is a healthy one to be having. The sharing of ideas and opinions is the cornerstone of the open source philosophy.

But I believe the debate may be premature.

Imagine a true IaaS stack as a tree. Strong trees must have a strong trunk to support their many branches. For IaaS technology, the trunk is built of essential cloud core services: compute, networking and storage. In OpenStack, these equate to Nova, Neutron, Cinder and Swift (or Ceph). The branches then consist of everything else that evolves the offering and makes it more compelling and easier to use: everything that builds upon the strong foundation of the trunk. In OpenStack, these branches include services like Ceilometer, Heat, Trove and Marconi.

I consider API compatibility a branch.

Without a robust, reliable sturdy trunk, the branches become irrelevant, as there isn't a strong supporting foundation to hold them up. And if neither the trunk, nor the branches are reliable, then the API to talk to them certainly isn't relevant.

In is my belief that OpenStack needs concentrate on strengthening the trunk before putting significant emphasis into the possibilities that exist in the upper reaches of the canopy.

OpenStack's core is quite close. Grizzly was the first release many would define as "stable" and Havana is the first release where that stability could convert into operational simplicity. But there still is room for improvement (particularly with projects like Neutron), so it is my argument to focus on strengthening the core before exploring new projects.

Once the core is strong then the challenge becomes the development of the service catalogue. Amazon has over one hundred different services that can be integrated together into a powerful ecosystem. OpenStack's service catalogue is still very young and evolving rapidly. Focus here is required to ensure this evolution is effective.

Long term, I certainly believe API compatibility with AWS (or Azure, or GCE) can bring value to the OpenStack ecosystem. Early cloud adopters who took to AWS before alternatives existed have technology stacks written to interface directly with Amazon's APIs. Being able to provide compatibility for those prospects means preventing them from having to rewrite large sections of their tooling to work with OpenStack.

API compatibility provided via a higher-level proxy would allow for the breakout of maintenance to a specific group of engineers focused on that requirement (and remove that burden from the individual service teams). It's important to remember that chasing external APIs will always be a moving target.

In the short run, I believe it wise to rally the community around a common goal: strengthen the trunk and intelligently engineer the branches.

_**[SL]** What are your thoughts on public vs. private OpenStack?_

**[JP]** For many, OpenStack draws much of its appeal from the availability of both public, hosted private and on-premise private implementations. While "cloud bursting" still lives more in the realms of fantasy than reality, the power of a unified API and service stack across multiple consumption models enables incredible possibilities.

Conceptually, public cloud is generally better defined and understood than private cloud. Private cloud is a relatively new phenomenon, and for many has really meant advanced virtualization. While it's true private clouds have traditionally meant on-premise implementations, hosted private cloud technologies are empowering a new wave of companies who recognize the power of elastic capabilities, and the value that single-tenant implementations can deliver. These organizations are deploying applications into hosted private clouds, seeing the value proposition that can bring.

A single-sourced vendor or technology won't dominate this world. OpenStack delivers flexibility through its multiple consumption models, and that only benefits the customer. Customers can use that flexibility to deploy workloads to the most appropriate venue, and that only will ensure further levels of adoption.

_**[SL]** There's quite a bit of discussion that private cloud strictly a transitional state. Can you share your thoughts on that topic?_

**[JP]** In 2012, we began speaking with IT managers across our customer base, and beyond. Through those interviews, we confirmed what we now call the "six tenets of private cloud." Our customers and prospects are all evolving their cloud strategies in real time, and are looking for solutions that satisfy these requirements:

1. Ease of use  new solutions should be intuitively simple. Engineers should be able to use existing tooling, and ops staff shouldn't have to go learn an entirely new operational environment.

2. Deliver IaaS and PaaS - IaaS has become a ubiquitous requirement, but we repeatedly heard requests for an environment that would also support PaaS deployments.

3. Elastic capabilities - the desire to the ability to grow and contract private environments much in the same way they could in a public cloud.

4. Integration with existing IT infrastructure  businesses have significant investments in existing data center infrastructure: load balancers, IDS/IPS, SAN, database infrastructure, etc. From our conversations, integration of those devices into a hosted cloud environment brought significant value to their cloud strategy.

5. Security policy control  greater compliance pressures mean a physical "air gap" around their cloud infrastructure can help ensure compliance and ease peace of mind.

6. Cost predictability and control - Customers didn't want to need a PhD to understand how much they'll owe at the end of the month. Budgets are projected a year in advance, and they needed to know they could project their budgeted dollars into specific capacity.

Public cloud deployments can certainly solve a number of these tenets, but we quickly discovered that no offering on the market today was solving all six in a compelling way.

This isn't a zero sum game. Private cloud, whether it be on-premise or in a hosted environment, is here to stay. It will be treated as an additional tool in the toolbox. As buyers reallocate the more than $400 billion that's spent annually on IT deployments, I believe we'll see a whole new wave of adoption, especially when private cloud offerings address the six tenets of private cloud.

_**[SL]** Thanks for your time, Jesse!_

If anyone has any thoughts to share about some of the points raised in the interview, feel free to speak up in the comments. As Jesse points out, debate can be healthy, so I invite you to post your (courteous and professional) thoughts, ideas, or responses below. All feedback is welcome!
