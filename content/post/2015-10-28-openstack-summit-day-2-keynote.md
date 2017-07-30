---
author: slowe
categories: Liveblog
comments: true
date: 2015-10-28T00:00:00Z
tags:
- OpenStack
- OSS
title: OpenStack Summit 2015 Day 2 Keynote
url: /2015/10/28/openstack-summit-day-2-keynote/
---

Mark Collier, COO of the OpenStack Foundation, takes the stage to kick things off. He starts with a story about meeting new people, learning new things, and sharing OpenStack stories, and encourages attendees to participate in all of these things while they are here at the Summit.

Mark then transitions into a discussion of Liberty (the latest release), and revisits Jonathan Bryce's discussion of the new organizational model ("the Big Tent"). He specifically calls out Astara and Kuryr as new projects in the Big Tent model. Out of curiosity, he looked at development activity for all the various projects to see which project was the "most active". It turns out that Neutron was the most active project across all of the various OpenStack projects. According to the user survey last year, 68% were running Neutron. In the most recent user survey, that number climbed to 89%---meaning the vast majority of OpenStack clouds in production are now running Neutron.

So why is networking (and Neutron) so hot right now? Mark believes that this is due to the increasing maturity of software-defined networking and network virtualization. Mark shows data from Crehan Research that states SDN is growing twice as fast as server virtualization. (Growth is only a single perspective, though---the overall adoption rate of server virtualization is still far, far higher.) Network Functions Virtualization (NFV) is a related area that, according to Mark, is ripe for disruption, especially in telco and carrier environments. According to data from ACG Research, the use of NFV can reduce capital expenditures by 68% and reduce operating expenditures by 67%. Additionally, NFV will dramatically shrink the time required to deploy new services (from 15 months down to 6 months). Naturally, Mark believes that OpenStack is a core part of this NFV revolution. Why? Because OpenStack serves the critical role as an "integration engine," that allows various disparate technologies, projects, and products to be pulled together.

Going back to Neutron, Mark calls out a few key features added in the Liberty release:

* RBAC (role-based access control) - this enables more fine-grained controls over creating, changing, removing, and sharing networks
* IPAM (IP address management) - Neutron now features a pluggable IPAM framework
* QoS (Quality of Service) APIs

This leads Mark into bringing Kyle Mestery on the stage to demonstrate some of the changes that were put into place in Neutron. Kyle starts off by again thanking all the contributors who helped with Neutron in the Liberty release cycle. He continues by providing a bit of history on the Neutron project (formerly called Quantum), providing some milestones on the project. This leads Kyle into a review of the overall design goals for Neutron, and he reviews basic Neutron abstractions, API extensions, and advanced network services (LBaaS, VPNaaS, and FWaaS). He also alludes to new projects in the works, like layer 2 gateways, service function chaining (SFC), and BGP MPLS VPN. Octavia is another project; it's the reference implementation for the v2 LBaaS API. Naturally, the topic of containers comes up, and Kyle believes that networking is the piece that ties all this together. This leads to a discussion of libnetwork, Docker's pluggable network architecture, and a comparison of libnetwork's abstractions and Neutron's abstractions, and how Kuryr maps libnetwork onto Neutron to provide integrated networking for Docker containers.

Kyle starts up a quick demo of Kuryr, which---unfortunately---doesn't work quite as expected. After a recap of what was _supposed_ to happen during the demo, Kyle gives the stage back to Mark Collier.

Mark introduces Toshio Nishiyama from NTT Resonant, who takes a few minutes to talk about what NTT is doing with Neutron in their OpenStack implementation, and why NTT chose OpenStack.

Following Toshio's presentation, Mark takes the stage to wrap up some of his comments regarding Neutron, SDN, NFV, containers, etc., and ties the comments back to the "OpenStack-powered planet" theme that's been bandied around this week.

Next up, Mark introduces a video by Rackspace, who is slated to take the stage following the video. Scott Crenshaw and Adrian Otto take the stage to talk about what Rackspace is doing, especially in conjunction with Intel regarding the OpenStack Innovation Center. Scott and Adrian talk about Carina, Rackspace's new container-as-a-service offering

Next up is Kang-Wong Lee from SK Telecom, who talks about SK's efforts around SDN, NFV, and OpenStack. (By the way, "SK" doesn't stand for South Korea, which is where SK Telecom originates. Kang-Wong doesn't tell us what it stands for.) Kang-Wong talks about SK's history of innovation, and this leads into a discussion of SK's work around developing 5G connectivity and the benefits it is anticipated to offer. He then spends quite a bit of time talking about how they envision networks evolving in the near future, and some of the technologies and techniques (virtual network slices, for example) that will be employed in such networks. In particular, he calls out OpenStack (of course), ONOS, and OVS-DPDK as key technologies that SK Telecom will be leveraging in future network buildouts.

Mark comes back on stage, and quickly transitions to a couple of folks from Rakuten, a Tokyo-based e-commerce and Internet company. The speakers are Neal Sato and Kentaro Sasako. They talk about why they chose OpenStack, although they were clear in stating that Rakuten was still early in its OpenStack journey.

Following Rakuten's presentation, Mark introduces Makoto Hasegawa from CyberAgent, who takes some time to talk about how CyberAgent is using OpenStack.

Mark Collier takes the stage again to introduce a couple of folks from IBM (Angel Luis Diaz and Jesse Proudman), but I had to step out to take care of some other things here at the Summit so I couldn't liveblog their portion of the morning keynotes.
