---
author: slowe
categories: Liveblog
comments: true
date: 2015-06-22T23:30:00Z
aliases: /2015/06/23/docker-networking/
tags:
- Docker
- DockerCon2015
- Networking
title: 'Liveblog: Docker Networking'
url: /2015/06/22/docker-networking/
---

This is a liveblog of the Docker Networking breakout session. This session is led by Madhu Venugopal and Jana Radhakrishnan, both formerly of Socketplane (and now with Docker following the acquisition). They are introduced by John Willis, also formerly of Socketplane and well-known within the DevOps community.

Some display issues plague the session at the beginning, so it appears that Murphy's Law is back with a vengeance.

Madhu starts out the session with an overview of why networking (in particular Docker networking) is so important. Networking is vast and complex, and networking is an inherent part of distributed applications. Therefore, it's important to make networking developer-friendly and application-driven. He shares a vision: "We'll do for networking what Docker did for compute". So what are the goals from this vision?

* Make "network" and "service" top-level objects
* Provide a pluggable networking stack
* Span networks across multiple hosts
* Support multiple platforms

Libnetwork is a key part of this effort. It was open-sourced in April, with over 200 pull requests and 200 GitHub stars. Windows and FreeBSD ports are in progress. Libnetwork is part of the Docker 1.7 release with limited functionality, allowing users to test it before it is fully enabled in a future release (1.8, I think).

Madhu now passes it over to Jana, who will do a "deeper dive" on Docker networking. Madhu mentioned Libnetwork, but Jana provides a more comprehensive explanation of what Libnetwork is. Libnetwork is a library for creating and managing network stacks for containers, intended to be independent of Docker. Libnetwork uses driver-based networking. "Driver-based networking" means that Libnetwork should (and can) support plugins to allow it to work with multiple back-ends from multiple providers. Via driver-based networking, Libnetwork provides a unified API for integrating networking solutions into Docker. Finally, Libnetwork implements the Container Network Model (CNM).

The CNM contains a number of different constructs:

* Endpoint (you might also see this referenced as service or service endpoint)
* Network
* Sandbox

Using the term "endpoint" allows Libnetwork to decouple the container from the connection to the network. The endpoint's lifetime is completely separate from the container's lifetime; endpoints can be created and destroyed independently of containers that provide a service via an endpoint. A "network" is a collection of endpoints that are allowed to communicate with each other. If a container is not part of a network, it can't (by default) communicate with containers that are part of a network. A "sandbox" is an isolation mechanism for grouping endpoints that is independent of the actual container. This makes Libnetwork more indedendent of Docker (not tied to the container runtime) and decouples the grouping of interfaces from the container itself. Using the idea of a sandbox also makes Libnetwork more OS-agnostic, since namespaces and the like are Linux constructs.

Now that he has covered the basic abstractions, Jana takes the audience through the workflow. First, you create a network. This pulls in the driver to actually "plumb" the network using whatever methods/means/technologies are available. Next, you create a container. When the container spawns, it defers to the driver to create an endpoint (service). A sandbox is created, and the endpoint is pushed into the sandbox. The sandbox is handed over to the Docker runtime, which uses it to create the container runtime. The end result is that all the networking services/connectivity are already plumbed before the container is launched. This addresses some race conditions when containers come up before networking is ready.

Some of the Libnetwork APIs include:

* libnetwork.New
* controller.ConfigureNetworkDriver
* controller.NewNetwork
* network.CreateEndpoint
* endpoint.Join

Jana points out that the "controller" shouldn't be confused with SDN controllers; this is a process that runs with every Docker daemon and is responsible for coordinating network connectivity. There is a northbound RESTful API available as well for integration with other orchestration tools.

The driver API is intentionally very narrow and focused; this helps with portability and support across multiple drivers (not all drivers will implement all features, so keeping the API narrow and focused is preferable). This is essentially "lowest common denominator."

True to Docker's "batteries included but swappable" mentality, Libnetwork ships with a Bridge Driver that re-implements the original Docker Linux bridge functionality. (This is what he meant when he said earlier that 1.7 includes Libnetwork in "reduced functionality" in order to test and harden the code.) In the experimental channel you'll find the Overlay driver, which uses network namespaces, iptables rules for NAT, veth pairs, Linux bridges, and VXLAN tunnels to connect containers across multiple hosts.

Service discovery is built into Libnetwork as well. Out of the box, Docker 1.8 (in the experimental channel) provides DNS-based service discovery. Future releases will provide plugins for service discovery.

And speaking of plugins, Libnetwork supports network plugins to extend networking functionality. Plugins will use JSON-RPC transport, can be written in any language, and can even be deployed as a container if so desired. This is how tools like OVS/OVN would integrate into Libnetwork.

At this point, Madhu takes the stage again to provide a live demo of how Docker Network will work when it is fully released in Docker 1.8. Fortunately, Murphy's Law steers clear of the session, and the demo works as expected (he creates a multihost overlay network between multiple Docker hosts and is able to connect over that network).

Madhu wraps up the session with a quick shout-out to some of the Docker networking ecosystems, including Weave, Nuage, Cisco, Microsoft, Calico, Midokura, and VMware.

At this point, Madhu and Jana open up for questions and the session wraps up.
