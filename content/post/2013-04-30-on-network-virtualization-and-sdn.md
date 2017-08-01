---
author: slowe
categories: Rant
comments: true
date: 2013-04-30T18:52:12Z
slug: on-network-virtualization-and-sdn
tags:
- Networking
- SDN
- Virtualization
title: On Network Virtualization and SDN
url: /2013/04/30/on-network-virtualization-and-sdn/
wordpress_id: 3162
---

Is there a difference between network virtualization and Software-Defined Networking (SDN)? If so, what is the relationship between them? Is one a subset of the other? This is a topic that is increasingly being discussed and debated. So, in a similar fashion to my post on [network overlays vs. network virtualization][1], I thought I'd weigh in with some thoughts. This post, like all of my posts, is intended to help spark a conversation, so I encourage you to share your thoughts in the comments after you've finished reading.

Last week I had the opportunity to join John Troyer on the VMware Communities podcast. My purpose, as John put it when he invited me, was to "gently introduce" the community to the idea of network virtualization, which is where I now spend most of my time since [joining VMware in early February][2]. One of the topics we discussed on the podcast was the relationship between SDN and network virtualization, which sparked [this blog post](http://virtualizedgeek.com/2013/04/24/networkvirtualizationvssdn/) by Keith Townsend, one of the attendees. I encourage you to read Keith's full blog post, but I think the key point he makes is right here:

>How is this different from Software Defined Networking or SDN? I think VMware (who Scott works for) would like you to consider SDN as just the abstraction of the control plane from the physical plane_**I believe the industry outside of VMware is defining SDN in a broader sense.**_ _(emphasis mine)_

I'll get to the definition of SDN in just a moment, but first let's look at the definition of network virtualization. In my earlier post on [network overlays vs. network virtualization][1], I provided a definition (taken from [a presentation Dan Wendlandt gave][3] at the OpenStack Summit in Portland) for network virtualization: _"A faithful and accurate reproduction of the physical network that is fully isolated and provides both location independence and physical network state independence."_ Several of the readers of that particular blog post then came up with a definition of a virtualized network: _"A virtualized network is one which can be instantiated, operated, and removed without physical asset interaction by the network manager."_

These definitions are, in my humble opinion, reasonably precise and accurate. They are easy to understand and easy to explain to others who aren't familiar with the concept of network virtualization.

With this definition in hand, let's compare network virtualization to SDN. To compare network virtualization to SDN, we must first define SDN. So---how do we define SDN?

There are two definitions we can use for SDN:

1. We can use the original definition when SDN was first coined in 2009.

2. We can use the definition currently in use within the industry.

Let's look at the first scenario, and let's examine SDN and network virtualization in the context of the original definition of SDN. I've had some discussions with Martin Casado, the so-called "Father of SDN" (he's going to hate me for using that term---sorry Martin!), about what SDN meant when it was first coined. According to Martin, the term SDN originally referred to a change in the network architecture to include a) decoupling the distribution model of the control plane from the data plane; and b) generalized rather than fixed function forwarding hardware.

Bruce Davie (Principal Engineer at VMware and long-time networking guru) recently talked with some of the other creators of OpenFlow in preparation for his presentation at ONS 2013. According to the research that Bruce did, the term "SDN" was first coined in 2009 to refer specifically to the work being done on OpenFlow. Their original definition of SDN is consistent with Martin's definition---it refers to the decoupling of the control plane from the data plane.

Therefore, if we look at the original definition of SDN---not VMware's definition, but the definition of those involved in the creation of the term in 2009--it becomes fairly clear that SDN is not the same as network virtualization. Instead, SDN is a tool that can be leveraged in order to create network virtualization. SDN is a tool or a technique that can be used in the creation of virtualized networks and in satisfying the definition of network virtualization. This subtle distinction, by the way, is one that Bruce addressed in [his recent ONS 2013 presentation](http://networkheresy.com/2013/04/29/netvirt-delivering/).

Now, let's talk about the second scenario, and let's examine SDN and network virtualization in the context of the current industry definition of SDN. The problem that we have here is that "SDN," like "cloud," can really be made to mean just about anything. Think about it: existing protocols like OSPF and BGP (which run in _software_) manipulate the forwarding rules in the data plane---does that make them SDN, too? What about NX-OS, JUNOS, or EOS? These are all software---does that make them SDN? What about having APIs on switches? What about virtualized load balancers? Or virtualized firewalls? Are these SDN too? What about products that existed long before the SDN term was coined in 2009? Are they now SDN? No, we generally don't accept all the vast assortment of software that's involved in networking today as SDN, because SDN originated as a term to refer to the separation of the control plane from the data plane. I think it's telling when Martin Casado indicates that [he doesn't even know what SDN means anymore](http://www.enterprisenetworkingplanet.com/netsp/openflow-inventor-martin-casado-sdn-vmware-software-defined-networking-video.html), the term is probably being overused. SDN-washing, anyone?

Therefore, we can't take the current industry definition of SDN, because SDN currently means all things to all people. If your company is doing "something cool" in networking, it's probably going to be called SDN. There is no clear, crisp, understandable definition.

This lack of clarity around "what is SDN vs. what isn't SDN" is part of the reason, I think, that VMware has converged on the use of network virtualization. This isn't just about differentiating itself from all the other SDN players out there, but it's also about giving customers something concrete and precise they can understand.

Now, if we want to---as an industry---come up with a clearer and more precise definition of SDN, then we can revisit this discussion. In fact, I encourage you to speak up in the comments below with what you might consider would be a useful, clear, precise definition of SDN. Clear definitions and better understanding enable meaningful communication, which can only be beneficial for everyone.

[1]: {{< relref "2013-04-16-network-overlays-vs-network-virtualization.md" >}}
[2]: {{< relref "2013-01-25-new-year-new-challenges-new-opportunities.md" >}}
[3]: {{< relref "2013-04-15-openstack-summit-2013-nicira-nvp-deep-dive.md" >}}
