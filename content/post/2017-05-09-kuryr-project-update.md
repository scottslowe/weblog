---
author: slowe
categories: Liveblog
comments: true
date: 2017-05-09T00:00:00Z
tags:
- OpenStack
- Docker
- Networking
title: 'Liveblog: Kuryr Project Update'
url: /2017/05/09/kuryr-project-update/
---

This is a liveblog of an OpenStack Summit session providing an update on the Kuryr project. The speakers are Antoni Segura Puimedon and Irena Berezovsky. Kuryr, if you recall, was a project aimed at making OpenStack Neutron functionality available to Docker containers; it has since expanded to also offer Cinder and Manila storage to Docker containers, and has added support for both Docker Swarm and Kubernetes as well.<!--more-->

According to Puimedon, the latest release of Kuryr has a diverse base of contributors, with over 45 active contributors.

So, what will be in the Pike release? For the Kubernetes-specific support:

* This will be the first release
* Support for Kubernetes Services (this leverages LBaaS v2)
* Client- and server-side SSL support
* RDO packaging

What's planned for Pike, but may not actually make it? (Again, this is for Kubernetes support.)

* Token support
* Resource pools
* Improved support for Services defined as LoadBalancer type

On the Docker side, the following new features and enhancements will arrive in Pike:

* Support for Swarm mode
* IPv4 and IPv6 networking
* TLS support between Docker and the libnetwork plugin

On the Fuxi side, Kuryr is adding support for Manila shares.

At this point, Berezovsky takes over to discuss the release themes for Kuryr as part of the Pike release:

* Scalability
* Modularity
* Interoperability
* Minor areas of focus included resiliency, manageability, security, and the overall user experience (UX)

Berezovsky provided examples of each of these areas, but those examples and their details were not shared on the slides so I wasn't able to capture all of them in this liveblog. She also runs through a similar discussion for the Queens release, but again none of the details were shared via the slides. I did pick up that they plan to switch LBaaS v2 to Octavia from HAProxy, but there were many details shared that I was unable to capture here.

A few Queens enhancements that were called out on the slides include:

* Support Kubernetes Network Policy semantics by mapping to Neutron security groups
* Supporting inbound connections access to cluster services
* Supporting Octavia as a service provider (I mentioned this already)

Next, Berezovsky reviews the Kuryr plans for the Rocky release. Obviously, the Rocky release is still a good ways out, so these plans may change over time as things progress.

At this point the session wraps up.

**UPDATE:** Antoni Segura Puimedon (one of the speakers) sent me [this URL][link-1] with more information on focus areas for the project.



[link-1]: https://docs.google.com/document/d/138Smryl2Bg8JD2AzGN_9RjtHZQ32dTxwkDMWblcRWH8/edit?usp=sharing
