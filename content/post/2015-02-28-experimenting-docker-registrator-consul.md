---
author: slowe
categories: Information
comments: true
date: 2015-02-28T16:00:00Z
tags:
- Docker
- Networking
- DNS
title: Experimenting with Docker, Registrator, and Consul
url: /2015/02/28/experimenting-docker-registrator-consul/
---

Over the last few days, I've been experimenting with [Docker][link-1], [Registrator][link-2], and [Consul][link-3] in an effort to explore some of the challenges involved in building a robust containerized infrastructure. While I haven't finished fully exploring the idea (and documenting what I've learned), I did discover one interesting---and unexpected---interaction.

Here's a quick overview of my testing environment:

* I used two OpenStack Heat templates to spin up two clusters of 5 instances each.
* The first cluster is a set of [CoreOS Linux][link-4] instances, customized via cloud-init to _not_ run etcd. These instances are attached to a [VMware NSX][link-5]-powered logical network using IP addresses from the 10.1.1.0/24 subnet.
* On each CoreOS Linux instance, I have Registrator running as a Docker container and listening to the Docker socket (thus listening to Docker events).
* The second cluster is a set of Ubuntu 14.04 instances running Consul. These instances are connected to an NSX-powered logical network using IP addresses from the 10.1.2.0/24 subnet.
* The two logical networks are connected by a logical router and thus have full connectivity.

Registrator, if you're not already familiar with it, is a service registry tool that listens to the Docker socket and registers/de-registers containers with a backend registry service (like Consul). This makes it an essential part of a dynamic containerized infrastructure, as service registration and service discovery are key challenges that must be addressed.

Based on some earlier testing, what I _expected_ to happen in this environment was this:

1. I would fire up a Docker container on one of the CoreOS instances.
2. Registrator running on that instance detects the new container and registers it with Consul (via one of the instances running Consul). The IP address registered with Consul would be the IP address of the CoreOS instance where the container is running (something from 10.1.1.0/24).
3. Services/applications/users could then discover (and access) the Docker container via the CoreOS instance's IP address by querying Consul (either via HTTP API or via DNS).

However, what I found _actually_ happened was this:

1. A new Docker container is launched on one of the CoreOS instances.
2. Registrator detects the new container and registers it with Consul. However, because Consul is running on a separate node, the IP address that gets registered _is the IP address of the Consul node itself_, not the CoreOS host instance.
3. Services/applications/users attempting to discover the new service think it resides on the Consul node (something on 10.1.2.0/24), when in reality it is running on a CoreOS instance.

I did not expect this behavior. From what I've been able to find (see [this issue][link-6]) this is expected behavior from Consul, and the only fix is to run Consul as a Docker container itself (a so-called "sidecar" container) on every host where Registrator is running. This is the configuration I used in my first round of testing, and thus why I was caught off-guard by the incorrect registration behavior in the new environment. In the next iteration of the testing environment, I'll run Consul as a sidecar container, although I need to deal with bootstrapping the Consul cluster in an environment where CoreOS instances will automatically reboot due to system updates.

I still have more testing to do (and more learning to do), and I'll continue to share the results of my testing/learning here.



[link-1]: https://www.docker.com
[link-2]: https://github.com/gliderlabs/registrator
[link-3]: https://www.consul.io
[link-4]: https://coreos.com
[link-5]: https://www.vmware.com/products/nsx/
[link-6]: https://github.com/gliderlabs/registrator/issues/59
