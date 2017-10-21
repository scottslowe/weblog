---
author: slowe
categories: Liveblog
comments: true
date: 2017-10-18T12:00:00Z
tags:
- Docker
- DockerCon2017
title: Docker EE Deep Dive
url: /2017/10/18/docker-ee-deep-dive/
---

This is a liveblog of the session titled "Docker EE Deep Dive," part of the Docker Best Practices track here at DockerCon EU 2017 in Copenhagen, Denmark. The speaker is Patrick Devine, a Product Manager at Docker. I had also toyed with the idea of attending the Cilium presentation in the Black Belt track, but given that I attended a version of that talk in Austin in April ([liveblog is here][xref-1]), I figured I'd better stretch my boundaries and dig deeper into [Docker EE][link-1].<!--more-->

Devine starts with a bit of information on his background, then provides an overview of the two editions (Community and Enterprise) of Docker. (Recall again that Docker is the downstream product resulting from the open source Moby upstream project.) Focusing a bit more on Docker EE, Devine outlines some of the features of Docker EE: integrated orchestration, stable releases for 1 year with support and maintenance, security patches and hotfixes backported to all supported versions, and enterprise-class support.

So what components are found in Docker EE? It starts with the Docker Engine, which has the core container runtime, orchestration, networking, volumes, plugins, etc. On top of that is Universal Control Plane (UCP), which provides the higher-level management functions. UCP managers are typically clustered, and they leverage the Raft consensus protocol for clustering (Raft is the protocol behind etcd, for example). UCP workers are the nodes on which you actually will deploy containerized workloads. Finally, you also have the Docker Trusted Registry (DTR), which can also run in a high-availability mode; DTR provides the registry/repository for storing container images.

Next, Devine moves into discussing security scanning, one of the features of Docker EE. Image scanning actually occurs at the binary level; it's not just looking at package versions but going through every single file. This functionality works both online (has Internet access) and offline (without Internet access, good for air-gapped installations). Scanning is supported for both Linux (x86_64) and Windows images (with support for IBM z Series in the works).

Under the covers, scanning works along 4 steps. First, it gets the image layers (you can also see this information using `docker history`). Next, it generates a bill of materials; this is really just a JSON file containing details for the components located within each of the layers DTR found. Also found in the bill of materials are the vulnerabilities associated with the components in the layers.

Image signing (Docker Content Trust) is integrated directly into DTR as well. Devine doesn't spend much time talking about image signing; instead, he points attendees to a separate session occurring later today (Wednesday 10/18) that focuses on image signing specifically.

The next feature of DTR that Devine discusses is image distribution (comprising image caching, image promotion, and image mirroring [an unreleased feature]). First introduced in DTR 2.2, Docker introduced image caching. DTR 2.3 included image promotion, and image mirroring is something "in the works".

Devine now provides a few more details about some of these image distribution features:

* Image caching can be compared to a content distribution network (CDN) for Docker images by caching layers closer to where the layers are being consumed (pulled). It works globally for all repositories in DTR, and preserves access permissions.
* Image promotion is about moving images between repositories in the same DTR (for example, moving from "dev" to "staging" to "qa" to "production"). Promotion can be done manually, or handled automatically via a policy. Images can be re-tagged as part of the promotion process, and the repositories each have their own access control. Policies can be written to leverage a variety of criteria (tags, vulnerabilities [or lack thereof], package presence or version, size, or even software license). Promotion policies can also be "chained" (promoting from dev to QA and then to production), even support branching out to multiple repositories.
* Finally, image mirroring is similar to image promotion in that it works on "blessed" images, but this time across registries (promotion is across repositories within a registry). Registries maintain their own access control, and mirroring is bi-directional (supporting push, pull, and webhook-initiated mirroring topologies). Policies can be used to push to remote DTR instances. Image signing and image scanning data will _not_ be preserved across registries. (Devine reminds everyone that this is not yet available in DTR, but is coming soon.)

Devine switches to perform a demo of the new image mirroring functionality. Because the UI/UX for mirroring isn't yet complete, Devine uses DTR's RESTful API to configure the mirroring between two DTR instances (for the demo, these are VMs running---under [VMware Fusion][link-2], it appears---on Devine's laptop).

Devine closes out the session by letting users know about a free 4 hour demo of Docker EE that's available, in case attendees are interested in trying out any of the features that were discussed in the session. Following some Q&A, Devine ends the session.



[link-1]: https://www.docker.com/enterprise-edition
[link-2]: https://www.vmware.com/products/personal-desktop-virtualization.html
[xref-1]: {{< relref "2017-04-18-black-belt-cilium.md" >}}
