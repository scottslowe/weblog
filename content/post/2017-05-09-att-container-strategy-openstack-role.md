---
author: slowe
categories: Liveblog
comments: true
date: 2017-05-09T00:00:00Z
tags:
- OpenStack
- Docker
- Kubernetes
title: 'Liveblog: AT&T''s Container Strategy and OpenStack''s Role in it'
url: /2017/05/09/att-container-strategy-openstack-role/
---

This is a liveblog of the OpenStack Summit session titled "AT&T's Container Strategy and OpenStack's Role in it". The speakers are Kandan Kathirvel and Amit Tank, both from AT&T. I really wanted to sit in on Martin Casado's presentation next door (happening at the same time), but as much as I love watching/hearing Martin speak, I felt this like presentation might expose me to some new information.<!--more-->

Kathirvel kicks off the session with some quick introductions, then sets the stage for the session. Naturally, Kathirvel starts out by describing AT&T's cloud deployment. (I say "naturally" because it seems that _every_ presentation starts out with describing how great and how awesome the presenter's company's OpenStack cloud is.)

Following the discussion of AT&T's cloud, Kathirvel launches into a discussion of container trends and demands. He indicates that he believes container usage (or demand?) for enterprise IT applications is huge (and will continue to be large), but doesn't believe that will hold true for virtual network functions (VNFs) in telco clouds.

As for how containers and OpenStack may be coming together, Kathirvel describes three different use cases:

1. The first use case has OpenStack managing the infrastructure, with Kubernetes (or another container orchestration tool) installed on top of the infrastructure.

2. The second use case has Kubernetes managing the OpenStack control plane.

3. The third use case has OpenStack (including Magnum) orchestrating both virtual machines (VMs) and containers directly.

Use cases #1 and #3 are tenant use cases (how tenants may see OpenStack and containers operating together), whereas #2 is more of a provider/infrastructure use case.

Tank now takes over to describe some ways containers and OpenStack will intersect. I think that this is intended to be a more in-depth discussion of the use cases that Kathirvel described earlier, although it's not clear if that is indeed the case. Tank does go into a bit more detail on the use case of running the OpenStack control plane in containers, though he glosses over some details that (in my humble opinion) are critically important. For example, Tank implies that upgrading OpenStack (or downgrading OpenStack) is easy when the control plane is containerized; I'm not so sure that's the case (what about inter-project interoperability across multiple versions?).

This brings Tank to describe what he calls a "container sandwich":

* A bare metal operating system (OS) instance.
* One or more containers running on this bare metal OS instance.
* A Libvirt instance running inside a container. This Libvirt instance can run "traditional" KVM VMs.

This is an industry combination (and one that I've heard before), but Tank does not provide a clear (to me) reason what tangible benefits there are to the "container sandwich" architecture. I can see benefits of running the OpenStack control plane in containers, but that seems to be separate and distinct from the "container sandwich" architecture that Tank is describing.

Kathirvel now takes over to describe some of the benefits that AT&T expects to see (has seen?) from running the OpenStack control plane in containers:

1. Less control plane overhead
2. Modular scaling
3. Better resiliency and large-scale support
4. Hitless/in-place upgrade
5. Better tools for deployment

Some of these benefits are accurate and make sense; modular scaling (and therefore large-scale support as a result) is something I can certainly see. I'm not so sure about hitless/in-place upgrades, but perhaps I'm missing something.

At this point, Kathirvel pivots slightly to talk about OpenStack plus Kubernetes plus Helm. I believe Kathirvel is suggesting that this should be the direction that the OpenStack community should take when it comes to running the OpenStack control plane in containers.

Kathirvel now starts summarizing what's been discussed, and then opens up for questions from the audience. The first question brings up the topic of why you should run a Libvirt instance inside a container (a good question, in my opinion). Tank steps forward to discuss the "Libvirt-in-a-container" question, stating that the value of the container is gaining access to a container orchestration engine for the service plus the strong isolation that virtualization provides. Tank believes that gaining access to the orchestration solution makes up for the added complexity of the "container sandwich" architecture. Kathirvel and Tank also addressed questions of "why Kubernetes instead of Docker Swarm," pointing to Kubernetes' significant mindshare and adoption versus Docker Swarm.

After a few minutes of questions, the speakers close out the session.
