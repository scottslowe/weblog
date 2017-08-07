---
author: slowe
categories: Liveblog
comments: true
date: 2017-05-09T00:00:00Z
tags:
- OpenStack
- OSS
- Interoperability
title: 'Liveblog: OpenStack Summit Keynote, Day 2'
url: /2017/05/09/openstack-summit-keynote-day-2/
---

This is a liveblog of the day 2 keynote of the OpenStack Summit in Boston, MA. (I wasn't able to liveblog yesterday's keynote due to a schedule conflict.) It looks as if today's keynote will have an impressive collection of speakers from a variety of companies, and---judging from the number of laptops on the stage---should feature a number of demos (hopefully all live).<!--more-->

The keynote starts with the typical high-energy video that's intended to "pump up" the audience, and Mark Collier (COO, OpenStack Foundation) takes the stage promptly at 9am. Collier re-iterates a few statistics from yesterday's keynote (attendees from 63 countries, for example). Collier shares that he believes that all major challenges humanity is trying to solve counts on computing. "All science is computer science," according to Collier, which is both great but also represents a huge responsibility. He leads this discussion by pointing out what he believes to be the fundamental role of open source in machine learning and artificial intelligence (ML/AI). Collier also mentions a collection of "composable" open source projects that are leading the way toward a "cloud-native" future. All of these projects are designed in a way to be combined together in a "mix-and-match" sort of way to build technology stacks.

This leads Collier to discuss how OpenStack and how OpenStack's components can (should?) be used in more composable ways, and points to Swift as a project that has been very successful in running either with or without OpenStack. To be successful in that effort, though, Collier believes there are a couple challenges that must be addressed:

* Complexity
* NIH (Not Invented Here) Syndrome

Addressing these challenges really matters in order to make OpenStack more composable and more consumable. This leads Collier to introduce the first of some demos, which will show off using Ironic to turn up OpenStack on a small rack of DellEMC equipment sitting on the stage. The demo is led by Julia Kreger of IBM. The demo shows Ironic being used to turn up infrastructure for Kubernetes.

That, in turn, leads to John Griffith and Kendall Nelson taking the stage to show how you can use Docker to deploy Cinder in containers on top of the Kubernetes infrastructure that was deploying using Ironic. That enables them to create a persistent volume through Cinder. The demo runs into a few timing errors, but Collier manages to keep things moving along with good use of humor.

One key takeaway from the Cinder-in-containers demo is that there is an OpenStack project called "Loki" that helps make OCI-compatible container images of OpenStack services like Cinder and others. (It is these images, created by Loki, that Griffith and Nelson are using for their demo.)

Next up, Collier introduces Jakub Pavlik from Mirantis to show a live demo of a unified platform for VMs, containers, and bare metal workloads.

Following a successful demo, Collier introduces a video from Intel. After the video, Imad Sousou from Intel takes the stage. Sousou takes a few minutes to pimp Intel's sessions at the Summit before moving into the meat of his presentation, which focuses on the need for OpenStack and Intel's efforts around making OpenStack more enterprise-ready. Sousou reiterates Intel's commitment to OpenStack, which I suppose is a response to the recent news regarding Intel's withdrawal from the OpenStack Innovation Center (a joint effort with Rackspace). After a disjointed mention of a bunch of different Intel products, Sousou wraps up.

Collier takes the stage again and brings out Brian Stevens, CTO of Google Cloud. Collier and Stevens talk for a few minutes about topics like enterprise complexity, public cloud, open source, and Google's role in open source.

Next up is another demo; Collier brings out Alex Polvi from CoreOS and Spencer Kimball from Cockroach Labs. Per Kimball, CockroachDB is so named because it replicates itself and is really hard to kill. Polvi and Kimball run through a demo of CockroachDB and Kubernetes. The first part of the demo is pretty straightforward, but then Collier brings Brad Topol out. Topol and Collier bring out 15 different OpenStack distributions to replicate last year's Interop Challenge---this time adding in CoreOS and CockroachDB. The demo is successful; attendees are able to see Kubernetes pods running CockroachDB running across multiple clouds in multiple countries all join the same CockroachDB cluster.

After the interoperability demo, Collier brings out Dr. Clemens Hardewig of Deutsche Telekom (DT). Hardewig spends a few minutes talking about how DT has participated in the OpenStack community and their OpenStack experience, and how DT is leveraging that experience and expertise in DT's new OpenStack-based public cloud offering, Open Telekom Cloud. Hardewig believes that OpenStack has an advantage over public cloud providers in the form of hybrid support, which provides functional differentiation. OpenStack will never compete with the public cloud on scale, but Hardewig believes OpenStack's hybridity will help it compete on differentiation for a variety of use cases.

Collier takes the stage again. He talks about the next two summits (Sydney, Australia, and then Vancouver, British Columbia, Canada). Right at the end of the keynote, Collier introduces his special guest, Edward Snowden, who joins the conference via video/audio link (he obviously can't be here in Boston in person). Collier and Snowden talk for a few minutes about cloud computing, the risks and benefits of cloud computing, how OpenStack addresses the "silent vulnerability" of public clouds, morals and ethics for open source contributors, and the dynamics of security vulnerabilities and their interplay with open source.

After talking with Snowden for a few minutes, Collier wraps up the keynote.
