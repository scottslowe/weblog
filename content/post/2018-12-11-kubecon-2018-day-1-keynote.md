---
author: slowe
categories: Liveblog
comments: true
date: 2018-12-11T12:00:00Z
tags:
- KubeCon2018
- Kubernetes
title: KubeCon 2018 Day 1 Keynote
url: /2018/12/11/kubecon-2018-day-1-keynote/
---

This is a liveblog from the day 1 (Tuesday, December 11) keynote of KubeCon/CloudNativeCon 2018 in Seattle, WA. This will be my first (and last!) KubeCon as a Heptio employee, and looking forward to the event.<!--more-->

The keynote kicks off at 9:02am with Liz Rice, Technology Evangelist at Aqua Security. Rice welcomes attendees (back) to Seattle, and she shares that this year's event in Seattle is 8x the size of the same event in Seattle just two years ago. Rice also shares some statistics from other CNCF events around the world, stressing the growth of these events both in size and in the number of events happening worldwide.

Rice next shares some entertaining statistics about web site visits to kubernetes.io versus some other popular brands. (TL;DR: Kubernetes gets more web site visits than the Seahawks and Manchester United, but not as many as Starbucks.)

Moving on, Rice talks for a few minutes about the strategy or purpose behind the collection of projects that fall under the CNCF umbrella (to provide some of the important building blocks in the full stack of technologies to support cloud-native environments). At this point, Rice turns it over to [Michelle Noorali](https://twitter.com/michellenoorali), a key maintainer of Helm.

Noorali starts out with a quick introduction/overview of Helm, likening it to `yum` or `apt` for Linux packages. Noorali then reviews the history of Helm, provides a high-level description of the Helm governance model, and introduces the Helm Hub ([https://hub.helm.sh](https://hub.helm.sh))---a new, centralized way to search for and deploy Helm Charts. Helm v2.12.0 (the "Egg Nog" edition) was also recently released. This leads Noorali into a high-level description of some of the features planned for Helm v3, which will move to an entirely client-side architecture (no more Tiller, yay!). Chart Museum, an open source Helm chart repository server, also recently released an update. Noorali ends her section with a call to action to participate in the Helm community.

Rice returns to the stage, this time discussing the second of the three graduated projects in the CNCF, Prometheus. After a brief discussion of Prometheus, Rice shifts her focus to discuss recent updates on Fluentd and Fluent Bit. OpenTracing and Jaeger (Jaeger is an open source implementation of the specifications created by OpenTracing) are next up for Rice to review, and she shares that deploying Jaeger has gotten easier via the Jaeger Operator.

Rice introduces Matt Klein, Constance Caramanolis, and Jose Nino to talk about the third graduated project, Envoy. All three of these individuals are (apparently) with Lyft, where Envoy originated. Using the Lyft architecture as a framework, Caramanolis provides an overview of the need for Envoy before turning it over to Nino to provide an update on where Lyft stands now with Envoy deployed almost everywhere. Nino turns it over to Matt Klein, who provides context on Envoy in the larger ecosystem, and who speculates on why Envoy has become so popular in such a short period of time. Klein attributes Envoy's success to performance, reliability, a modern codebase, extensibility, best-in-class observability, and a strong configuration API.

Following the discussion of Envoy, Rice returns to the stage to continue to review the CNCF projects. She starts with CoreDNS, which in the 1.13 release becomes the default DNS project for Kubernetes deployments. Rice moves on to Linkerd 2.0, and shows a quick video demo of deploying Linkerd into Kubernetes and seeing some of the detailed statistics that Linkerd enables.

Moving into storage, Rice mentions Rook, which as of September moved from the Sandbox into the CNCF Incubator. Next up is Vitess, which is now at version 3.0 and sports a number of important new features. gRPC, a high-performance RPC framework based on HTTP/2, is the next project that Rice mentions. With regards to messaging, Rice mentions NATS, the next CNCF project in the list to be reviewed.

The last project Rice mentions is Harbor, a container registry that also recently moved from Sandbox to the CNCF Incubator, and which integrates with Notary for providing content trust/signing for container images.

Following a quick review of the sponsors, Rice introduces Brandon Philips and Xiang Li, who take the stage to talk about 5 years of etcd. (Etcd is the consensus platform underneath Kubernetes.) Philips provides a quick overview of etcd and the origins that drove the creation of etcd at CoreOS. Philips announces that etcd is being moved to the CNCF today. This is a good move, in my opinion, and one that was much-needed. Li takes over to review the development of etcd over the last five years, including etcd's own implementation of the Raft consensus protocol. Etcd itself leverages or integrates with other cloud-native technologies, like Prometheus, gRPC, and the Operator framework (not necessarily a CNFC project, but a common design pattern emerging in Kubernetes).

Li turns it back over to Philips to talk about the future of etcd in the CNCF. Key support items that the etcd community is looking forward to it more robust testing, support for etcd discovery service(s), and performing third-party security and correctness audits. Philips ends with a review of all the etcd-related sessions coming up at the conference.

Next, Janet Kuo, co-chair of the KubeCon track, takes the stage to introduce Aparna Sinha, the Group Product Manager for Kubernetes at Google. Sinha spends a few minutes re-iterating the success and growth of the Kubernetes community before moving on to new projects like Istio and Knative. Istio is an Envoy-based service mesh to provide observability, security, and control. Knative is a portable serverless framework that runs on top of Kubernetes (naturally). Sinha shows a recorded demo that shows GKE running with one-click installs of both Istio and Knative. (Sinha does point about that these projects are not limited to GKE, of course.)

Kuo comes back to the stage to introduce Wendy Cartee, Senior Director of Cloud Native Advocacy at VMware. Cartee reviews the lessons learned at VMware in reaching enterprises. Cartee also reviews some of VMware's new and expanded open source efforts.

Kuo returns to the stage to introduce Matt Butcher and Karen Chu, both with Microsoft. Butcher and Chu are presenting "Phippy Goes to the Zoo," an illustrated children's guide to Kubernetes. After reading the book and going over the process for creating the book, Butcher and Chu announce that Phippy and all characters are being donated to the CNCF. Rice adds that the CNCF is licensing Phippy and the characters as Creative Commons, which allows people to re-use these characters in their own materials.

Another set of keynotes is happening this afternoon; if my schedule permits, I'll try to liveblog those keynotes as well.

At this point, Rice and Kuo wrap up the morning keynotes.
