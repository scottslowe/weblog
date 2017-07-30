---
author: slowe
categories: Liveblog
comments: true
date: 2012-09-13T13:19:03Z
slug: sfts012-designing-a-trusted-cloud-with-openstack
tags:
- IDF2012
- Intel
- OpenStack
- Security
title: 'SFTS012: Designing a Trusted Cloud with OpenStack'
url: /2012/09/13/sfts012-designing-a-trusted-cloud-with-openstack/
wordpress_id: 2864
---

This is session SFTS012, titled "Designing a Trusted Cloud with OpenStack." It is, unfortunately, my last session of the conference; I'm leaving from here to head to the airport to catch my flight home. The speaker for the session is Vin Sharma, who helps lead open source software strategies at Intel. The focus of this session, as you can tell from the title, is on security and trust (presumably talking about how to leverage Intel TXT with OpenStack).

Intel's "Cloud 2015" vision recognizes there will be more users, more devices, and more data, and is shooting toward an ecosystem of open, interoperable cloud operating environments (OEs) built on open APIs and open standards. (That's a lot of "open".)

Based on feedback from ODCA (Open Data Center Alliance) members, security issues are top of mind for cloud implementations. These issues are second only to concerns on how to migrate applications to cloud OEs (or, in Sharma's terms, "how to cloudify applications"). 20% of ODCA members included security-related issues as top challenges for cloud implementations.

Sharma reviews again some of the ODCA cloud computing usage models. In the security space, there are a number of different usage models available. Sharma will focus on the "Security Provider Assurance" usage model during this presentation.

The challenges in this space include:

* Proving compliance with an audit record

* Provide visibility or control over placement of workloads in the cloud

Sharma (and Intel) believes that "trusted compute pools" are the answer to these challenges. Using Intel technologies like TXT (Trusted Execution Technology) along with a policy engine and related components such as TPM, providers can build "trusted compute pools" to help solve security-related issues in a cloud OE.

The problem, according to Sharma, is that the natural evolution of open source projects---which are playing an increasingly influential role in the direction of data centers, cloud OEs, and provider implementations/services---is often at odds with what enterprises need. This means that there must be some sort of "driving force," such as a vendor or organization, that helps shape and focus open source development in the right development. Intel believes OpenStack is the right cloud OE, and believes that the support of OpenStack across the industry provides the right "shape and focus" to ensure that OpenStack starts to address enterprise data center needs. For this reason, Sharma states, Intel believes OpenStack is the right vehicle to address the ODCA usage models and the right place to implement trusted compute pools.

While Sharma believes OpenStack is the right vehicle, it still has a way to go. He shows a couple of slides that demonstrate that users expect a certain level of functionality, but OpenStack is (today) only prepared to deliver a subset of that functionality. This gap provides a number of opportunities, not only for Intel but also for other vendors. This is especially true in areas like auditing and security incident event management (SIEM). (RSA, are you listening?)

At the heart of enabling trusted compute pools is the scheduler, and that's where Intel started. The scheduler needs to be able to make intelligent decisions about where a VM should be provisioned, so that it can provision workloads on the basis of platform characteristics (is this a trusted host or an untrusted host, in this particular instance). While the initial changes to the scheduler are focused on trusted compute pools, there are additional directions to take the scheduler (power consumption, workload characteristics, and performance, for example). In order to be able to determine the trust status of a host, OpenStack needs an attestation service. This allows the scheduler to determine if a host is trusted/untrusted, and therefore be able to make intelligent scheduling decisions based on trust and security policy.

So how does this open attestation service that Intel has created work? It uses something called the Trousers stack (I'm not familiar with this one) and a host agent to determine trusted/untrusted status. The attestation server uses HTTPS to communicate with the host agent's API, and provides an API by which OpenStack can communicate with the attestation server (in order to check status). Sharma indicates that a white paper is under development that will provide more details on exactly how this is implemented. The OpenAttestation code is available on Github. The other components required to make this work will either be delivered in Folsom (where changes in the scheduler are available) or already in the Linux kernel (like the tboot functionality/support).

At this point Sharma wraps up the session.
