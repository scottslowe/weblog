---
author: slowe
categories: Liveblog
comments: true
date: 2017-11-05T23:00:00Z
tags:
- OpenStack
title: OpenStack Summit Sydney Day 1 Keynote
url: /2017/11/05/openstack-summit-sydney-day-1-keynote/
---

This is a liveblog of the day 1 keynote here at the [OpenStack][link-1] Summit in Sydney, Australia. I think this is my third or fourth trip to Sydney, and this is the first time I've run into inclement weather; it's cloudy, rainy, and wet here, and forecasted to remain that way for most of the Summit.<!--more-->

At 9:02am, the keynotes (there are actually a set of separate keynote presentations this morning) kicks off with a video with Technical Committee memebers, community members, and others talking about the OpenStack community, the OpenStack projects, and the Summit itself. At 9:05am, the founders of the Australian OpenStack User Group---Tristan Goode and Tom Fifield---take the stage to kick off the general session. Goode and Fifield take a few minutes to talk about the history of the Australian OpenStack User Group and the evolution of the OpenStack community in Australia. Goode also takes a few moments to talk about his company, Aptira.

After a few minutes, Goode and Fifield turn the stage over to Mark Collier and Lauren Sell from the OpenStack Foundation. Collier and Sell set the stage for the upcoming presentations, do some housekeeping announcements, and talk about sponsors and support partners. Sell also takes a moment to provide a quick introduction to OpenStack, since about 40% of the attendees are here at the Summit for the very first time. Collier mentions that there are over 60 different locations/regions where an OpenStack-powered public cloud has a presence. This leads Collier to talk about free trials of OpenStack-powered public clouds, called the OpenStack Passport (visit [https://openstack.org/passport/][link-2] for more information).

Collier brings out Monty Taylor, who talks about OpenStack Zuul (the integration testing system behind OpenStack, which is itself hosted on OpenStack). The latest release of Zuul, version 3, is now better designed to be run by other organizations (it was more closely tied to OpenStack in previous versions). Version 3 of Zuul leverages Ansible (instead of Jenkins Job Builder) and now has support for GitHub.

At this point, Collier brings out Jonathan Bryce, Executive Director of the OpenStack Foundation, to talk for a few minutes about the future of OpenStack. Bryce takes a few minutes to reflect on the history of OpenStack, looking back at the previous fifteen summits (this summit is the sixteenth summit). Bryce also reflects on the nature of OpenStack, looking at the four "opens" (open source, open design, open technology, and open community). This discussion leads Bryce to talk about the challenges facing open source projects; in particular, making sure open source innovation can be put to work by users. Bryce brings out Alison Randall to talk about how integration is a key effort to help put open source innovation to work for users and customers.

Randall talks for a few moments about the integration efforts happening within the OpenStack community, not just within the OpenStack projects but also among OpenStack projects and "external" open source projects. Randall revisits the "integration engine" theme used in the Austin Summit, talking about integration isn't just about technology, it's also about knowledge and information.

Bryce and Randall now bring up four steps on which the OpenStack Foundation will be focusing around integration:

1. Find the common use cases. (Putting logos on a slide doesn't make technologies work together.)
2. Collaborate across communities, building trust and establishing standardized interfaces. (The OpenDev event is one example.)
3. Build the required new technology, addressing the gaps identified in the previous steps.
4. Test everything end-to-end, making sure that the integration is real, stable, and reliable. (OPNFV and XCI are examples of this in action.)

Bryce's focus now shifts to organizing the OpenStack community, and how this is necessary in order to help support the four integration steps described above. Bryce encourages the OpenStack community to step up and focus on building an integration engine that helps users address their challenges.

At this point, Bryce hands the stage over to Sorabh Saxena from AT&T. Saxena leads off his presentation by talking about OpenStack's role in FirstNet, a network focused on supporting first responders. Saxena encourages the attendees and the community to focus on building a "next-generation OpenStack" that embodies the principles Bryce mentioned in his presentation; in particular, Saxena calls out the edge computing use case. Some AT&T services built on OpenStack are DirecTV Now and Cricket Wireless, according to Saxena. This leads Saxena to provide an update on AIC (AT&T Integrated Cloud), a solution build on OpenStack. Next, Saxena talks about key efforts that "next-generation" OpenStack should address: security, simplified operations, upgrades and installation, and culture and process. At the end of his talk, Saxena brings out a couple presenters (couldn't catch their names) to show a demo of some of the work AT&T is doing around the "next-generation" efforts Saxena described.

Saxena wraps up and Sell returns to the stage, highlighting a few other telecom-related sessions happening at the Summit this week. Sell then introduces Commonwealth Bank and Quinton Anderson to talk about their use of OpenStack.

Anderson talks for a moment about Commonwealth Bank, then focuses on some of the challenges users encounter when composing open source cloud technologies to address business needs. Anderson outlines Commonwealth Bank's use of Ironic to help with their Hadoop installations, and talks about using Docker/Vault/Calico under Mesos+Marathon to support Yarn, Spark, etc. Anderson goes into some detail on the CI/CD pipeline his organization is using to manage "infrastructure as code", and then talks about the collection of open source projects the bank has had to compose together. This list includes Jenkins, Notary, Vault, Nginx, Mesos, Kubernetes, Calico, Docker, Spark, Hadoop, Cassandra, and OpenStack.

Sell returns to the stage now, highlighting some financial services-related sessions happening at the summit this week.

Continuing the keynote presentations, Sell introduces Anni Lai from Huawei. Lai talks about the "three Internet giants" in China: Baidu, Alibaba, and Tencent. After a moment talking about Tencent's business, Lai introduces Bowyer Liu, who is doing a live demo of mobile access to the OpenStack Summit web site (not sure what the unique value of Tencent was in the demo). Liu then takes center stage to talk a few minutes about Tencent, fairly quickly transitioning to Tencent's use of OpenStack, called TStack. As an example, Liu talks about how WeChat Procurement---a payment system---runs on OpenStack and handles 200,000 orders daily. Tencent also offers an OpenStack-powered public cloud offering called Tencent Cloud (TStack is an internal private cloud running OpenStack.). Liu discusses other initiatives stemming from Tencent's efforts, including various government efforts and industry-specific cloud projects.

Collier now returns to the stage, recapping the Tencent presentation and now transitioning to the Superuser program. To that end, Collier brings out Allison Price and Nicole Martinelli. Price and Martinelli talk about the Superuser program, reviews the recent awards, and introduces Thomas Andrew from Paddy Power Betfair. Andrew announces the candidates for the next Superuser candidates: China Railway Corporation, China UnionPay, City Network, and the Tencent TStack team. The winner is the Tencent TStack team. Liu returns to the stage, along with Collier and the rest of the Tencent TStack team.

After some photos, Collier shifts the focus to savings realized via OpenStack, and brings out Joseph Sandoval and Nicolai (?) Brousse from Adobe Advertising Cloud. Cost efficiency was a big driver for Adobe, leading them to move from public cloud (where they'd started) to a private cloud using OpenStack. According to their internal measurements, Adobe realized a 30% savings in moving to an OpenStack-powered private cloud. However, the speakers reiterated the need for close alignment with the business, perseverance with the users who are consuming the private cloud/services, and retaining focus on delivering value to the business.

Collier returns to the stage to provide the Adobe team with "100K cores" stickers, and introduces the next speaker. The next speaker is Lew Tucker, VP and CTO for cloud computing from Cisco. Tucker's focus, based on the opening slide, is on multi-cloud architectures. Tucker reviews a few statistics that show the trend is clearly toward the use of multiple clouds, and shows a slide outlining all of the various efforts within Cisco that address multi-cloud architectures. However, challenges clearly remain that have not yet been addressed, and this leads Tucker to review Cisco's recent announcement with Google Cloud. Complexity around microservices architectures leads Tucker to talk about leveraging a service mesh---such as that provided by Istio---to help simplify some of the complexity around microservices-based architectures, and Tucker talks about Cisco's joint efforts with Google, Lyft, and others in building out Istio and Istio-based service meshes.

As Tucker wraps up his presentation, Sell returns to the stage to introduce the next set of speakers. Carrying on a tradition of hearing about OpenStack in science and research, Sell introduces Dr. Steven Manos and Dr. Steve Quenette to talk about Nectar, a national "research cloud" that spans seven locations across Australia. Nectar supports over 27,000 cores and 12,000 registered users. These speakers, in turn, introduce Brendan Mackey to talk about his research on climate change and biodiversity (his research uses a "virtual laboratory" powered by the Nectar Cloud). After Mackey's talk, the next speaker is introduced: Gary Eagan, who steps up to talk about brain research powered by OpenStack.

Collier and Sell return to the stage to wrap up the presentations, providing a "shout out" to the Science Working Group and their efforts in supporting OpenStack in science.



[link-1]: https://www.openstack.org/
[link-2]: https://openstack.org/passport/
