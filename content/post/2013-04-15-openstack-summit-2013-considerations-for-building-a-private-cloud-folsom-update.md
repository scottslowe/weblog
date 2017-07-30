---
author: slowe
categories: Liveblog
comments: true
date: 2013-04-15T16:18:58Z
slug: openstack-summit-2013-considerations-for-building-a-private-cloud-folsom-update
tags:
- OpenStack
title: 'OpenStack Summit 2013: Considerations for Building a Private Cloud, Folsom
  Update'
url: /2013/04/15/openstack-summit-2013-considerations-for-building-a-private-cloud-folsom-update/
wordpress_id: 3136
---

This is a live blog for the session titled "Considerations for Building a Private Cloud, Folsom Update," led by Ryan Richard of Rackspace ([@rackninja](https://twitter.com/rackninja) on Twitter). As with other sessions here at the 2013 OpenStack Summit, this session is totally full, with people standing in the back, sitting on the floor along the sides, and seated on the floor across the front.

This session is about design considerations for building a private cloud with OpenStack. The focus will be the Folsom release. This session is based on experience after running Folsom for 6 months. Ryan intends to be able to provide a Grizzly-based version of this talk at the next time, after running Grizzly for 6 months.

First he tackles the question, "What is a private cloud?"

* Are you looking for elastic or traditional virtualization? It most likely won't be both.

* Multi-tenant (or, more likely, multi-application)

* Size (this talk will be limited to discussions of up to 100 nodes)

* Private endpoints (the management endpoints aren't accessible from the Internet)

* Limited inbound connectivity

* Customized for specific workloads

	Ryan's first recommendation is "Build with the end in mind." He looks at how deploying the "m1.tiny" flavor would create a mismatch between CPU and RAM utilization, in that 48 vCPUs will be utilized but only a fraction of the host's RAM would be allocated. The "m1.medium" flavor (4GB RAM, 2 vCPUs) creates a more balanced workload, whereas the larger flavors imbalance utilization the other way.

	What this tells me is that capacity planning is just as importan with a private cloud deployment as it is with a "traditional virtualization" solution. Ryan's recommendations around capacity are:

* Don't use a disk size of 0.

* For public cloud offerings, you can limit the number of flavors. For private cloud offerings, you can create "customized" flavors for specific workloads. Find a balance for your organization.

* Don't forget about network utilization.

It's important to remember that it's easy to add compute nodes, but you can't changed the fixed network (without Quantum) once instances are running. Quantum helps address this. (Note that Ryan indicates it's possible to create multiple networks using the CLI or API in Folsom without Quantum, but the dashboard doesn't respect or recognize it.)

This limit on the fixed network means that it is critically important to size the number of addresses available by calculating the number of instances that could be spun up within the cloud.

It's easy to add nodes or networks on the host network side, but you can't change the fixed network once you go into production (not without destroying your instances). It's also easy to add more floating networks.

Ryan now switches gears to talk about images and storage. (There's a session tomorrow in C123 at 1:50 PM.) Rackspace is going with virtio, qcow2 disk format, bare container, cloud-init with dynamic partitioning. I'm not 100% familiar with all of these terms, so you might expect to see some posts soon on some of these. There is a small performance hit with qcow2, so be sure to quantify that when building images (or re-using images that someone else has built).

Snapshotting is another sizing consideration that can't really be adequately predicted (it's hard to know if your users will be using snapshots or not). Ryan recommends qcow2 for snapshots. To help maximize the use of host caching of images, try to streamline the number of images.

A few more Glance tips:

* Watch for network utilization. Glance could consume an entire 1Gb NIC.

* Consider RAID-5 for large sequential reads/writes.

* Disk bandwidth is more important than disk IOPS.

* Reduce the number of images to improve host cache functionality.

Storage on the compute nodes, on the other hand, require a different view. Build for random I/O (RAID 10 or SSD or both).

A few architectural examples and thoughts:

* For 1-20 servers, a single controller, a single API, and a single network (1 to 2 Gbps) are probably sufficient. If you need HA, you'll need to increase those numbers appropriately.

* For 20-100 servers, take the same architecture but add load balancing for APIs (for HA and scalability), use Swift (or CloudFiles or S3) for Glance, consider the use of availability zones, and consider dedicated networks for management, Cinder, VM-to-VM traffic, etc. You might also want to consider a dedicated system for gathering compute node metrics.

Some other general performance considerations:

* Watch random IO and try to get as much random IO off local storage onto Cinder where possible.

* Review hypervisor best practices.

A few other "lessons learned":

* Floating IPs must be associated with the "public_interface"

* Each piece of OpenStack has its own architecture.

* Folsom is stable.

* Migration (live, block) works but scenarios exist where it doesn't. Try not to rely on these mechanisms where possible, especially if you're building an "elastic cloud" as opposed to "traditional virtualization."

* OpenStack is changing often, so keep up to date with the current state of the projects.

* Don't do heterogeneous nodes.

A few other operational updates:

* There are some new nova hypervisor calls

* New image types in Glance (including VMDK)

* The policy.json file

* Other things coming in Grizzly: cells, Quantum, and better AD/LDAP support

At this point Ryan opens the session up for Q&A.
