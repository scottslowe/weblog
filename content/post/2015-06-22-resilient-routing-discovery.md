---
author: slowe
categories: Liveblog
comments: true
date: 2015-06-22T23:00:00Z
aliases: /2015/06/23/resilient-routing-discovery/
tags:
- Docker
- DockerCon2015
title: 'Liveblog: Resilient Routing and Discovery'
url: /2015/06/22/resilient-routing-discovery/
---

This is a liveblog of the DockerCon 2015 session on resilient routing and discovery, part of the "Advanced Tech" track. Simon Eskilden ([@Sirupsen][link-1] on Twitter) from Shopify is the speaker for this session.

Not surprisingly (you'd understand this if you walked Eskilden's presentation from DockerCon EU 2015), he starts out with a mention of the walrus (his favorite animal). Eskilden starts with a brief overview of Shopify (his employer) and Shopify's production deployment of Docker (they've had Docker in production for over a year). Eskilden freely acknowledges that moving to a microservices-based architecture increases complexity and is not "free". In order to help address the complexity brought on by microservices-based architectures, Eskilden wants to talk about resiliency, service discovery, and routing.

Eskilden reinforces that companies shouldn't be implementing Docker solely for the sake of implementing Docker; it should be for a reason, a purpose (for him, it's making sure Shopify's services stay up and available). Resiliency is about building a reliable system from a bunch of unreliable components. Total availability is the availability per service to the power of the number of services. This means that the more services there are, the lower the total availability is. (To help understand this, realize that 100 services who individually have 99.99% availability will, together, have about 90% availability.)

One very important and powerful optimization is that applications should be designed to provide a fallback level of functionality. The failure of a single service should not cause the entire application to be unavailable; instead, limited functionality should continue to be available. 

One way of testing the behavior of application is to use a Shopify project called Toxiproxy (available at [https://github.com/shopify/toxiproxy][link-2]). This allows users to test all the various parts of an application that might have failed, be unreachable, or be responding slowly. Slowness is the killer in distributed systems---a refused connection is a luxury. Beating Little's Law is the first priority as you add services (Little's Law says that as latency goes up, throughput goes down).

Eskilden next provides an overview of the "resiliency pyramid"---going from no resiliency efforts to application-specific fallbacks all the way up to the Region Gorilla (part of the Netflix Simian Army).

This leads into a discussion of discovery. There are three sources of data that may be useful in discovery: services, metadata, and orchestration. Will discovery data be limited to a regional scope, or a global scope? Eskilden provides a list of desirable properties for a discovery backbone:

* No single point of failure
* Stale reads are better than no reads (A &gt; C)
* Reads occur orders of magnitude larger than writes
* Fast convergence can be important (in some cases)

What kinds of solutions are available? There are a lot: DNS, Chef, Puppet, network-level protocols, hardcoded values (these are the "old school" methods), as well as tools like Consul, etcd, Eureka, ZooKeeper (these are the "new school" methods). The new school methods may not be as battle-hardened and reliable as the old school methods.

For Shopify, pure DNS worked really, really well. Eskilden recommends using DNS as long as possible. For many, many people, using a new system won't outweigh the added complexity. DNS is resilient, simple, and supported, and often can be API-driven. It does have some drawbacks: failovers, slow convergence, not a data store, not for orchestration. Eskilden shares that Shopify found they only really needed high-level global discovery and orchestration---not anything more complex than that. Shopify also needed a way to route shops to the right data center, although this is something they haven't rolled out yet (coming later this year, according to Eskilden).

Shopify settled on ZooKeeper because Shopify felt was ZooKeeper was the most mature, most stable, and most battle-tested of the "new school" discovery solutions. However, ZooKeeper is not without its drawbacks. It's not a complete discovery solution, the clients are complex, and there is significant operational overhead involved in managing a ZooKeeper cluster.

The final topic Eskilden touches on is routing (what some people call networking). He shares a complex comparison matrix showing various solutions (Nginx, HAProxy, Vulcand, Finagle, etc.), but it ends up that Shopify settled on DNS. The final Shopify solution combines DNS, Chef, and ZooKeeper (ZK). The servers connect to ZK through a ZK proxy to discover load balancers. Ultimately, Shopify plans to remove Chef from the discovery layer and add more resiliency to the routing layer.

Eskilden wraps up the session with a reminder to the attendees: Be careful, be sure to map out what you _really_ need. Don't overcompensate! Reliability is your metric. Build resiliency into the system, don't make it optional, and make sure the infrastructure teams own the integration points.

At this point, the session wraps up.


[link-1]: https://twitter.com/sirupsen
[link-2]: https://github.com/shopify/toxiproxy
