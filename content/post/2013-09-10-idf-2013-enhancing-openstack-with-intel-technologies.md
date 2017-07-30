---
author: slowe
categories: Liveblog
comments: true
date: 2013-09-10T17:07:51Z
slug: idf-2013-enhancing-openstack-with-intel-technologies
tags:
- Hardware
- IDF2013
- Linux
- Networking
- OpenStack
- Storage
- Virtualization
title: 'IDF 2013: Enhancing OpenStack with Intel Technologies'
url: /2013/09/10/idf-2013-enhancing-openstack-with-intel-technologies/
wordpress_id: 3282
---

This is a liveblog of Intel Developer Forum (IDF) 2013 session EDCS003, titled "Enhancing OpenStack with Intel Technologies for Public, Private, and Hybrid Cloud." The presenters are Girish Gopal and Malini Bhandaru, both with Intel.

Gopal starts off by showing the agenda, which will provide an overview of Intel and OpenStack, and then dive into some specific integrations in the various OpenStack projects. The session will wrap up with a discussion of Intel's Open IT Cloud, which is based on OpenStack. Intel is a Gold Member of the OpenStack Foundation, has made contributions to a variety of OpenStack projects (tools, features, fixes and optimizations), has built its own OpenStack-based private cloud, and is providing additional information and support via the Intel Cloud Builders program.

Ms. Bhandaru takes over to provide an overview of the OpenStack architecture. (Not surprisingly, they use the diagram prepared by Ken Pepple.) She tells attendees that Intel has contributed bits and pieces to many of the various OpenStack projects. Next, she dives a bit deeper into some OpenStack Compute-specific contributions.

The first contribution she mentions is Trusted Compute Pools (TCP), which was enabled in the Folsom release. TCP relies upon the Trusted Platform Module (TPM), which in turn builds on Intel TXT and Trusted Boot. Together with the Open Attestation (OAT) SDK (available from https://github.com/OpenAttestation/OpenAttestation), Intel has contributed a "Trust Filter" for OpenStack Compute as well as a "Trust Filter UI" for OpenStack Dashboard. These components allow for hypervisor/compute node attestation to ensure that the underlying compute nodes have not been compromised. Users can then request that their instances are scheduled onto trusted nodes.

Intel has also done work on TCP plus Geo-Tagging. This builds on TCP to enforce policies about where instances are allowed to run. This includes a geo attestation service and Dashboard extensions to support that functionality. This work has not yet been done, but is found in current OpenStack blueprints.

In addition to trust, Intel has done work on security with OpenStack. Intel's work focuses primarily around key management. Through collaboration with Rackspace, Mirantis, and some others, Intel has proposed a new key management service for OpenStack. This new service would rely upon good random number generation (which Intel strengthened in the Xeon E5 v2 release announced earlier today), secure storage (to encrypt the keys), careful integration with OpenStack Identity (Keystone) for authentication and access policies, extensive logging and auditing, high availability, and a pluggable-backend (similar to Cinder/Neutron). This would allow encryption of Swift objects, Glance images, and Cinder volumes. The key manager project is called Barbican (https://github.com/cloudkeep/barbican) and provides integration with OpenStack Identity. In the future, they are looking at creation and certification of private-public pairs, software support for periodic background tasks, KMIP support, and potential AES-XTS support for enhanced performance. This will also leverage Intel's AES-NI support in newer CPUs/chipsets.

Intel also helped update the OpenStack Security Guide (http://docs.openstack.org/sec/).

Next, Intel talks about how they have worked to expose hardware features into OpenStack. This would allow for greater flexibility with the Nova scheduler. This involves work in libvirt as well as OpenStack, so that OpenStack can be aware of CPU functionality (which, in turn, might allow cloud providers to charge extra for "premium images" that offer encryption support in hardware). The same goes for exposing PCI Express (PCIe) Accelerator support into OpenStack as well.

Gopal now takes over and moves the discussion into storage in OpenStack. With regard to block storage via Cinder, Intel has incorporated support to filter volumes based on availability zone, capabilities, capacity, and other features so that volumes are allocated more intelligently based on workload and type of service required. By granting greater intelligence to how volumes are allocated, cloud service providers can offer differentiated (read: premium priced) services for block storage. This work is enabled in the Grizzly release.

In addition to block storage, many OpenStack environments also leverage Swift for object storage. Intel is focused on enabling erasure coding to Swift, which would enable reduced storage requirements in Swift deployments. Initially, erasure coding will be used for "cold" objects (objects that aren't accessed or updated frequently); this helps preserve the service level for "hot" objects. Erasure coding would replace triple replication to reduce storage requirements in the Swift capacity tier. (Note that this something I also discussed with SwiftStack a couple weeks ago during VMworld.)

Intel has also developed something called COSBench, which is an open source tool that can be used to measure cloud object storage performance. COSBench is available at https://github.com/intel-cloud/cosbench.

At this point, Gopal transitions to networking in OpenStack. This discussion focuses primarily around Intel Open Network Platform (ONP). There's another session that will go deeper on this topic; I expect to attend that session and liveblog it as well.

The networking discussion is _very_ brief; perhaps because there is a dedicated session for that topic. Next up is Intel's work with OpenStack Data Collection (Ceilometer), which includes work to facilitate the transformation and collection of data from multiple publishers. In addition, Intel is looking at enhanced usage statistics to affect compute scheduling decisions (essentially this is utilization-based scheduling).

Finally, Gopal turns to a discussion of Intel IT Open Cloud, which is a private cloud within Intel. Intel is now at 77% virtualized, with 80% of all new servers being deployed in the cloud. It's less than an hour to deploy instances. Intel estimates a savings of approximately $21 million so far. Where is Intel IT Open Cloud headed? Intel IT is looking at using all open source software for Intel IT Open Cloud (this implies that it is not built with open source software today). There is another session on Intel IT Open Cloud tomorrow that I will try to attend.

At this point, Gopal summarizes all of the various Intel contributions to OpenStack (I took a picture of this I posted via Twitter) and ends the session.
