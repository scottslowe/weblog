---
author: slowe
categories: Liveblog
comments: true
date: 2014-09-11T11:19:37Z
slug: idf-2014-open-source-storage-optimizations
tags:
- Hardware
- IDF2014
- Intel
- OSS
- Storage
title: 'IDF 2014: Open Source Storage Optimizations'
url: /2014/09/11/idf-2014-open-source-storage-optimizations/
wordpress_id: 3529
---

This is a liveblog of IDF 2014 session DATS009, titled "Ceph: Open Source Storage Software Optimizations on Intel Architecture for Cloud Workloads." (That's a mouthful.) The speaker is Anjaneya "Reddy" Chagam, a Principal Engineer in the Intel Data Center Group.

Chagam starts by reviewing the agenda, which---as the name of the session implies---is primarily focused on Ceph. He next transitions into a review of the problem with storage in data centers today; specifically, that storage needs "are growing at a rate unsustainable with today's infrastructure and labor costs." Another problem, according to Chagam, is that today's workloads end up using the same sets of data but in very different ways, and those different ways of using the data have very different performance profiles. Other problems with the "traditional" way of doing storage is that storage processing performance doesn't scale out with capacity, storage environments are growing increasingly complex (which in turn makes management harder).

Chagam does admit that not all workloads are suited for distributed storage solutions. If you need high availability and high performance (like for databases), then the traditional scale-up model might work better. For "cloud workloads" (no additional context/information provided to qualify what a cloud workload is), distributed storage solutions may be a good fit. This brings Chagam to discussing Ceph, which he describes as the "only" (quotes his) open source virtual block storage option.

The session now transitions to discussing Ceph in more detail. RADOS (stands for "Reliable, Autonomous, Distributed Object Store") is the storage cluster that operates on the back-end of Ceph. On top of RADOS there are a number of interfaces: Ceph native clients, Ceph block access, Ceph object gateway (providing S3 and Swift APIs), and Ceph file system access. Intel's focus is on improving block and object performance.

Chagam turns to discussing Ceph block storage. Ceph block storage can be mounted directly as a block device, or it can be used as a boot device for a KVM domain. The storage is shared peer-to-peer via Ethernet; there is no centralized metadata. Ceph storage nodes are responsible for holding (and distributing) the data across the cluster, and it is designed to operate without a single point of failure. Chagam does not provide any detailed information (yet) on how the data is sharded/replicated/distributed across the cluster, so it is unclear how many storage nodes can fail without an outage.

There are both user-mode (for virtual machines) and kernel mode RBD (RADOS block device) drivers for accessing the backend storage cluster itself. Ceph also uses the concept of an Object Store Daemon (OSD); one of these exists for every HDD (or SSD, presumably). SSDs would typically be used for journaling, but can also be used for caching. Using SSDs for journaling would help with write performance.

Chagam does a brief walkthrough of the read path and write path for data being read from or written to a Ceph storage cluster; here is where he points out that Ceph (by default?) stores three copies of the data on different disks, different servers, potentially even different racks or different fault zones. If you are writing multiple copies, you can configure various levels of consistency within Ceph with regard to how writing the multiple copies are handled.

So where is Intel focusing its efforts around Ceph? Chagam points out that Intel is primarily targeting low(er) performance and low(er) capacity block workloads as well as low(er) performance but high(er) capacity object storage workloads. At some point Intel may focus on the high performance workloads, but that is not a current area of focus.

Speaking of performance, Chagam spends a few minutes providing some high-level performance reviews based on tests that Intel has conducted. Most of the measured performance stats were close to the calculated theoretical maximums, except for random 4K writes (which was only 64% of the calculated theoretical maximum for the test cluster). Chagam recommends that you limit VM deployments to the maximum number of IOPS that your Ceph cluster will support (this is pretty standard storage planning).

With regard to Ceph deployment, Chagam reviews a number of deployment considerations:

* Workloads (VMs, archive data)

* Access type (block, file, object)

* Performance requirements

* Reliability requirements

* Caching (server caching, client caching)

* Orchestration (OpenStack, VMware)

* Management

* Networking and fabric

Next, Chagam talks about Intel's reference architecture for block workloads on a Ceph storage cluster. Intel recommends 1 SSD per every 5 HDD (size not specified). Management traffic can be 1 Gbps, but storage traffic should run across a 10 Gbps link. Intel recommends 16GB or more of memory for using Ceph, with memory requirements going as high as 64GB for larger Ceph clusters. (Chagam does not talk about why the memory requirements are different.)

Intel also has a reference architecture for object storage; this looks very similar to the block storage reference architecture but includes "object storage proxies" (I would imagine these are conceptually similar to Swift proxies). Chagam does say that Atom CPUs would be sufficient for very low-performance implementations; otherwise, the hardware requirements look very much like the block storage reference architecture.

This brings Chagam to a discussion of where Intel is specifically contributing back to open source storage solutions with Ceph. There isn't much here; Intel will help optimize Ceph to run best on Intel Architecture platforms, including contributing open source code back to the Ceph project. Intel will also publish Ceph reference architectures, like the ones he shared earlier in the presentation (which have not yet been published). Specific product areas from Intel's lineup that might be useful for Ceph include Intel SSDs (including NVMe), Intel network interface cards (NICs), software libraries (Intel Storage Acceleration Library, ISA-L), software products (Intel Cache Acceleration Software, or CAS). Some additional open source projects/contributions are planned but haven't yet happened. Naturally Intel partners closely with Red Hat to help address needs around Ceph development.

ISA-L is interesting, and fortunately Chagam has a slide on ISA-L. ISA-L is a set of algorithms optimized for Intel Architecture platforms. These algorithms, available as machine code or as C code, will help improve performance for tasks like data integrity, security, and encryption. One example provided by Chagam is improving performance for SHA-1, SHA-256, and MD5 hash calculations. Another example of ISA-L is the Erasure Code plug-in that will be merged with the main Ceph release (it currently exists in the development release).

Virtual Storage Manager (VSM) is an open source project that Intel is developing to help address some of the management concerns around Ceph. VSM will primarily focus on configuration management. VSM is anticipated to be available in Q4 of this year.

Intel Cache Acceleration Software (CAS) is another product (not open source) that might help in a Ceph environment. CAS uses SSDs and DRAM to speed up operations. CAS currently really only benefits read I/O operations.

Finally, Chagam takes a few minutes to talk about some Ceph best practices:

* You should shoot for one HDD being managed by one OSD. That, in turn, translates to 1GHz of Xeon-class computing power per OSD and about 1GB of RAM per OSD. (Resource requirements are pretty significant.)

* Jumbo frames are recommended. (No specific MTU size provided.)

* Use 10x the default queue parameters.

* Use the deadline scheduler for XFS.

* Tune `read_ahead_kb` for sequential reads.

* Use a queue depth of 64 for sequential workloads, and 8 for random workloads.

* For small clusters, you can co-locate Ceph monitoring process with compute workloads, but it will take 4-6GB of RAM.

* Use dedicated nodes for monitoring when you move beyond 100 OSDs.

Chagam now summarizes the key points and wraps up the session.
