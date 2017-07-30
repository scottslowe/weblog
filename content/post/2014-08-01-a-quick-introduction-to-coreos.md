---
author: slowe
categories: Education
comments: true
date: 2014-08-01T09:00:00Z
slug: a-quick-introduction-to-coreos
tags:
- Docker
- Linux
- OpenStack
- CoreOS
title: A Quick Introduction to CoreOS
url: /2014/08/01/a-quick-introduction-to-coreos/
wordpress_id: 3482
---

In this post, I'm going to provide a very quick introduction to [CoreOS](http://www.coreos.com/). CoreOS, in case you haven't heard of it, is a highly streamlined Linux distribution designed with containers, massive server deployments, and distributed systems/applications in mind.

CoreOS is built around a number of key concepts/technologies:

  1. _The OS is updated as a whole, not package-by-package._ CoreOS uses [the Omaha protocol](https://code.google.com/p/omaha/)--initially engineered by Google for updating things like the Chrome browser and Chrome OS---to stay up-to-date with new versions. CoreOS also employs an active/passive dual root partition scheme. This dual root partition scheme allows CoreOS to run off one root partition while updating the other; the system then reboots onto the updated partition once an update is complete. If the system fails to boot from the updated partition, then reboot it again and it will revert to the known-good installation on the first partition.

  2. _All applications run in containers._ CoreOS provides out-of-the-box support for [Docker containers](https://www.docker.com/). In fact, all applications on CoreOS run in containers. This enables separation of applications from the underlying OS and further streamlines the CoreOS update process (because applications are essentially self-contained).

  3. _CoreOS leverages systemd._ [systemd](http://freedesktop.org/wiki/Software/systemd/) is not unique to CoreOS; it is the new standard system and service manager for Linux. (Debian has elected to use systemd; Ubuntu will adopt systemd with 14.10, if I understand correctly; and Red Hat and related distributions already use systemd.) In CoreOS, systemd unit files are used not only for system services, but also for running Docker containers.

  4. _CoreOS has a distributed key-value data store called etcd._ The etcd distributed key-value data store can be used for shared configuration and service discovery. etcd uses a simple REST API (HTTP+JSON) and leverages the [Raft consensus protocol](http://raftconsensus.github.io/). Docker containers on CoreOS are able to access etcd via the loopback interface, and thus can use etcd to do dynamic service registration or discovery, for example. etcd is also configurable via cloud-init, which means it's friendly to deployment on many cloud platforms including [OpenStack](https://coreos.com/docs/running-coreos/platforms/openstack/). More information on etcd is available via [the etcd GitHub site.](https://github.com/coreos/etcd/)

  5. _CoreOS supports deploying containers across a cluster using fleet._ [Fleet](https://github.com/coreos/fleet/) is another open source project that leverages etcd to deploy Docker containers (written as systemd unit files) across a cluster of CoreOS systems. Fleet leverages both etcd and systemd to support the deployment of containers across a cluster of systems. See [this page](https://coreos.com/using-coreos/clustering/) for more information on clustering with CoreOS and fleet.

Taken individually---the use of a minimal Linux distribution, systemd support, the distributed key-value data store, Docker support, dual root partition w/ recoverable system updates, fleet---these technologies are interesting, but not all that revolutionary. Put them all together, however, and you have (in my opinion) a very interesting solution.

I'm quite intrigued with CoreOS and do plan on spending more time with it in the near future, so stay tuned for additional posts. In the meantime, if you'd like to see something specific about CoreOS or any related technologies, please speak up in the comments. I'll do my best to satisfy your requests!
