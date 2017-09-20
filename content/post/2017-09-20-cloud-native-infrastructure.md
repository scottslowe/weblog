---
author: slowe
categories: Liveblog
comments: true
date: 2017-09-20T20:00:00Z
tags:
- HashiConf2017
- Microsoft
- Kubernetes
title: 'Liveblog: Cloud Native Infrastructure'
url: /2017/09/20/liveblog-cloud-native-infrastructure/
---

This is a liveblog of the HashiConf 2017 session titled "Cloud Native Infrastructure." The speaker is Kris Nova, a Senior Developer Advocate at Microsoft. Kris, along with Justin Garrison, authored the O'Reilly _Cloud Native Infrastructure_ book (more information [here][link-1]). As one of the last sessions (if not the last session) I'll be able to attend, I'm looking forward to this session.<!--more-->

Kris is a self-confessed Linux lover, loves writing in Golang, is a Kubernetes maintainer, and works on Azure at Microsoft.

So, what is "cloud-native infrastructure"? To answer that, Nova first tries to answer "what is a cloud?" Nova breezes by that definition without going into any real detail (or any real definition), and proceeds to talk about what infrastructure is. Again, Nova breezes by that without providing any real definition or depth, and proceeds to ask "Why is infrastructure better in the cloud?" According to Nova, infrastructure is better in the cloud because management can be as simple as an HTTP request. The next few slides in Nova's presentation compare the "traditional" ways of managing infrastructure (provisioning switches, patching cables, troubleshooting problems) are now, when infrastructure is in the cloud, as simple as a series of `curl` commands (a bit oversimplified, but the idea is certainly sound).

Nova talks about a few different topics, including the OSI reference model, before finally arriving at the statement that everything is software. Nova talks about the different ways infrastructure has been represented over time (as a means of illustrating the move to software): infrastructure as a diagram, infrastructure as a script, infrastructure as code, and infrastructure as software. (Nova admits infrastructure as software is just a little extension of infrastructure as code.)

Nova spends a little bit of time talking about the etcd Operator as "software managing software," which then brings up the topic of Kubernetes. Nova boils Kubernetes down as software managing software. How? Users declare the state, and software (Kubernetes) reconciles the state (makes the actual state look like the desired state).

Focusing a bit on the "bootstrap problem," Nova talks about the creation of `gcc` (which was used to compile itself) as a means of solving the issue of configuring infrastructure by applications when applications need infrastructure in order to run (see the circular reference?). As a concrete example of using infrastructure as software (see earlier) to boot infrastructure as software, Nova shows off a hack that runs Terraform inside Kubernetes (aka `terraformctl`).

Wrapping up the presentation, Nova tells attendees to stop managing infrastructure the old way; it's time to recognize that infrastructure engineers are now software engineers.

With that, Nova ends the session.



[link-1]: http://www.cnibook.info/
