---
author: slowe
categories: Liveblog
comments: true
date: 2017-11-07T20:30:00Z
tags:
- OpenStack
- Virtualization
title: Lessons Learnt from Running a Container-Native Cloud
url: /2017/11/07/lessons-learned-running-container-native-cloud/
---

This is a liveblog of the session titled "Lessons Learnt from Running a Container-Native Cloud," led by Xu Wang. Wang is the CTO and co-founder of Hyper.sh, a company that has been working on leveraging hypervisor isolation for containers. This session claims to discuss some lessons learned from running a cloud leveraging this sort of technology.<!--more-->

Wang starts with a brief overview of Hyper.sh. The information for this session comes from running a Hypernetes (Hyper.sh plus Kubernetes)-based cloud for a year.

So, what is a "container-native" cloud? Wang provides some criteria:

* A container is a first-class citizen in the cloud. This means container-level APIs and the ability to launch containers without a VM.
* The cloud offers container-centric resources (floating IPs, security groups, etc.).
* The cloud offers container-based services (load balancing, scheduled jobs, functions, etc.).
* Billing is handled on a per-container level (not on a VM level).

To be honest, I don't see how _any_ cloud other than Hyper.sh's own offering could meet these criteria; none of the major public cloud providers (Microsoft Azure, AWS, GCP) currently satisfy Wang's requirements. A "standard" OpenStack installation doesn't meet these requirements. This makes the session more like a subtle but unmistakable plug for Hyper.sh, rather than a more genuine desire to share information learned on running containers on OpenStack at large scale. This impression is reinforced by a few slides in which Wang extolls the benefits of a container-native cloud as opposed to more "traditional" approaches, followed by a few more slides plugging Hyper.sh directly.

The next couple of slides also focus exclusively on Hyper.sh, and it appears that the majority of this session will be an advertisement for Hyper.sh (instead of focusing on helping the OpenStack community understand how OpenStack components and architecture may be affected by the extensive use of containers within an OpenStack deployment). At this point, I wrap up my liveblog.
