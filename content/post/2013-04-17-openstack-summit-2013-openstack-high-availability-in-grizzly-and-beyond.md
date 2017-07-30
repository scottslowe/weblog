---
author: slowe
categories: Liveblog
comments: true
date: 2013-04-17T19:06:44Z
slug: openstack-summit-2013-openstack-high-availability-in-grizzly-and-beyond
tags:
- OpenStack
- Storage
- Networking
title: 'OpenStack Summit 2013: OpenStack High Availability in Grizzly and Beyond'
url: /2013/04/17/openstack-summit-2013-openstack-high-availability-in-grizzly-and-beyond/
wordpress_id: 3153
---

This is a session titled "More Reliable, More Resilient, More Redundant: OpenStack High Availability in Grizzly and Beyond." The presenter is Florian Haas from Hastexo (@hastexo). Florian is one of the founders of Hastexo, which is a services firm that provides OpenStack services, among other things.

There are four things that need to be addressed when discussing high availability:

* Infrastructure

* Storage

* Compute

* Networking

The presentation starts with a discussion of the infrastructure layer. Changes from Folsom to Grizzly are relatively few. Examples of infrastructure services like the databases (typically MySQL, sometimes PostgreSQL), AMQP (message queue; could be using RabbitMQ, ZeroMQ, etc.), and API services. With regard to infrastructure HA, there are 5 types of infrastructure nodes:

1. Cloud controller

2. API node

3. Network node

4. Compute node

5. Storage controller

The cloud controller runs services that underpin OpenStack. It runs a relational database, an AMQP server, registry services, etc. The ability for an implementation to use active/passive or active/active depends largely on the specific back-end applications in use. Largely, cloud controllers will be deployed active/passive (this is due, in part at least, to the persistent data storage found in the relational databases).

API nodes are fundamentally stateless (locally); it interacts with the AMQP message bus. For most API services, we can use active/active scale-out approaches for API services.

The network node is, according to the presenter, "interesting." This node takes care of routing between tenant networks, provider networks, and upstream networks. The network node typically also runs the DHCP agent to provide IP addresses to tenant systems. We'll come back to the network node shortly.

The compute node is the hypervisor itself, where guest VMs/instances actually execute.

The storage controller depends greatly on what kind of back-end block storage. The block storage server (Cinder server) might have no local storage (which might be the case if you were running with Ceph, for example) or it might have lots of local storage.

All five of these infrastructure node types can use the same high availability stack, but with a few minor differences. The "recommended" high availability stack is Pacemaker. Hastexo has reference configurations for using Pacemaker with all OpenStack infrastructure services. According to the presenter, "it's not rocket science---you can do it."

The presenter next shows a diagram of an architecture using Pacemaker and Corosync for high availability of the cloud controller. From there he shows an example of using Pacemaker/Corosync for high availability of the stateless API services.

From here, he moves on to discuss HA for compute in a bit more detail. Grizzly addresses guest HA. He shows a hack where you "override" a couple of parameters to "trick" Nova into restarting VMs on another host after the first host fails. Unfortunately, this hack breaks live migration and is unsafe with Cinder volumes (although the Cinder issue is fixable).

In Grizzly, Nova has a `nova evacuate` command, but this command actually only works _after_ a host has failed. This evacuation functionality isn't present in Horizon (the Dashboard).

Another interesting feature is VM Ensembles. This feature allows you to group guests in a resilient fashion. The presenter provides an example of 6 VMs (2 each of three tiers), and the desire to make sure that the redundant nodes aren't running on the same compute node. (For VMware users, this would be analogous to anti-affinity rules.) Unfortunately, this feature did not make it into the Grizzly release (it should be in Havana). A workaround would be to use a scheduler filter.

Looking in more detail at Networking, the same HA architecture (Pacemaker/Corosync) worked reasonably well in Folsom for Quantum-server and L3 agent. It didn't work so well for the DHCP agent. However, this active/passive approach didn't scale well. Grizzly helps address this by running multiple DHCP agents and multiple L3 agents to provide better scale-out support (via the Quantum scheduler).

Finally, the presenter moves on to discuss storage. Grizzly has dramatically expanded the amount of support within Cinder for block storage options; the presenter highly recommends upgrading to Grizzly if you need expanded Cinder support. He briefly mentions new Cinder drivers for NFS, GlusterFS, 3Par, LeftHand, EMC, and others. Pacemaker/Corosync is needed for the Cinder volume server. There is a hack required (need to override a host value in the database) in order to provide high availability for the Cinder volume server. It's possible that this hack will be fixed in Havana (a bug has already been filed to fix it).

A few other tidbits:

* Libvirt watchdog support in Nova and Glance is coming

* Heat will bring some additional high availability features (Heat is now an integrated project and will be fully supported with Havana, as will Ceilometer)

* The RabbitMQ library (Kombu) has gained the ability to have a list of queue hosts to which to connect (instead of just a single host)

* ZeroMQ (peer-to-peer messaging) is another interesting option

* MySQL/Galera is firming up to provide write-set replication (which will enable synchronous replication)

* Some changes are occurring within RBD when you use it for both Glance and Cinder (the presenter did not elaborate)

At this point, the presenter wrapped up the session by opening up for questions and answers.
