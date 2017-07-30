---
author: slowe
categories: Liveblog
comments: true
date: 2014-09-09T17:46:09Z
slug: idf2014-bare-metal-docker-containers-and-virtualization
tags:
- Docker
- IDF2014
- Intel
- Linux
- Virtualization
title: 'IDF 2014: Bare Metal, Docker Containers, and Virtualization'
url: /2014/09/09/idf2014-bare-metal-docker-containers-and-virtualization/
wordpress_id: 3517
---

This is a live blog of session DATS004, titled "Bare-Metal, Docker Containers, and Virtualization: The Growing Choices for Cloud Applications." The speaker is Nicholas Weaver (yes, **that** Nick Weaver, who now works at Intel).

Weaver starts his presentation by talking about "how we got here", discussing the various technological shifts that have affected the computing landscape over the years. Weaver includes a discussion of the drivers behind virtualization as well as the pros and cons of virtualization.

That, naturally, leads to a discussion of containers. Containers are not all that new---Solaris Zones is a form of containers that existed back in 2004. Naturally, the recent hype associated with Docker has, according to Weaver, rejuvenated interest in the concept of containers.

Before Weaver gets too far into containers, he first provides a background of some of the core containerization pieces. This includes cgroups (the ability to control resource allocation/utilization), which is built into the Linux kernel. Namespace isolation is also important, which provides full process isolation (so that one process can't see processes in another namespace). Namespace isolation isn't just for processes; there's also isolation for network entities, mounts, and users. LXC is a set of user-space tools that attempted to make using these constructs easier, but it hasn't (until recently) been easy to really leverage these constructs.

Weaver next takes this relatively abstract discussion and makes it a bit more concrete with a specific example of how a microservice architecture would look under virtualization (OS instance, microservice libraries, and microservice itself) and well as under containers (shared OS instance and shared libraries plus microservice itself). Weaver talks about the "instant start" attribute of a container, but puts that in the context of the lifetime of the workload that's running in the container. Start-up times don't really matter for long-lived workloads, but for temporary, ephemeral workloads start-up times do matter. The pattern of "container on VM" is also mentioned by Weaver as another design pattern that some people use.

Next Weaver provides a quick list of pros and cons of containers:

* Pros: faster lifecycle vs. virtual machines; containers what is running within the OS; ideal for homogenous application stacks on Linux; almost non-existent overhead

* Cons: very complex to configure (by itself, absent some sort of orchestration system or operating at scale); currently much weaker security isolation than VMs; applications must run on Linux (because Windows doesn't have the same container technologies)

Next, Weaver transitions the discussion to focus on Docker specifically. Weaver describes Docker as "an easy button for containers," making the underlying containerization constructs (cgroups, namespaces, etc.) easier to use. Docker is simpler and easier than LXC (where multiple binaries were involved). Weaver believes that Docker images---which he describes as an ordered set of actions to build a container---are the real game-changer. Weaver's discussion of Docker images leads to a review of a Dockerfile, which is a DSL (domain specific language) for creating Docker images. Docker images are built on a series of layers; underlying layers could be "just" OS images (like Ubuntu or CentOS), but they could also be customized builds that contain applications and/or data.

Image registries are how users can create images and share images with other users. The public Docker Hub is an example of an image registry.

The discussion now transitions into a quick review of the underlying Docker architecture. There is a Docker daemon that runs on Linux; the Docker client can be run elsewhere. The Docker client communicates with the Docker daemon (although you should note that in many cases the daemon listens on a local socket, which means using a Docker client remotely over the network won't work).

The innovations that Weaver attributes to Docker include: images (like templates for VMs, and the use of copy-on-write makes them behave like code); API and CLI tools for managing container deployments; reduced complexity around deploying and managing containers; and support for namespaces and resource limits.

Weaver provides a more concrete example of how Docker can change a developer's process for creating code. Here Weaver's DevOps background really starts to show, as he discusses how Docker and containers would help streamline CI/CD operations.

Next up are the gotchas with containers. Trust is one gotcha; can we trust that one container won't affect other containers? The answer, according to Weaver, is "it depends." You still need to follow current recommended practices, such as no root access, host-level patches, auditing, and being aware of the default settings (which might be dangerous, if you aren't aware). One way to address some of these concerns is to use VMs to provide strong security isolation between containers that need a stronger level of isolation than the standard container mechanisms can provide.

Intel, of course, is working on making containers better:

* Security (Intel AES-NI, INtel TXT/TCP, Intel SGX)

* Performance/flexibility (Intel VT-x/VT-d/VT-c)

Weaver wraps up the session with a quick summary of the key points from the session and some Q&A.
