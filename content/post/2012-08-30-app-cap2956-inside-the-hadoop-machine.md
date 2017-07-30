---
author: slowe
categories: Liveblog
comments: true
date: 2012-08-30T14:53:26Z
slug: app-cap2956-inside-the-hadoop-machine
tags:
- OSS
- Virtualization
- VMware
- VMworld2012
title: 'APP-CAP2956: Inside the Hadoop Machine'
url: /2012/08/30/app-cap2956-inside-the-hadoop-machine/
wordpress_id: 2802
---

This is a session blog for APP-CAP2956, Inside The Hadoop Machine. Presenting are Jeff Buell (VMware), Richard McDougall (VMware), and Sanjay Radia (Hortonworks).

Some use cases for Hadoop include log processing, click stream analysis, machine learning, web crawling, and image/XML processing. But what is Hadoop? Hadoop is a programming framework for enabling parallel processing; this enables distributed processing of large data sets across clusters of computers. Hadoop also incorporates that machines will fail, and is tolerant of those failures. Hadoop works by taking an input file, breaking it up into pieces (the size of those pieces is driven by the HDFS block size), and giving those pieces out to the systems in the Hadoop cluster. The first phase is the map phase; then comes the reduce phase, where the results are sorted and merged, then given the output file. (Hence the name MapReduce.)

Hadoop consists of two pieces: MapReduce, which is the programming framework for parallel processing; and the Hadoop Distributed File System (HDFS), which provides distributed data storage for the MapReduce jobs. HDFS is not POSIX-compliant; it behaves more like Amazon S3 or EBS.

HDFS has two components: a name node and a data node. The name node is responsible for the directory hierarchy and the namespace. HDFS 1 only has a single name node; this represents a single point of failure for the filesystem. Data nodes store the actual data blocks, and provide replication for the blocks as well in the event of failure (typically 3 copies are made, and the third copy will be in a separate rack/fault domain).

Hadoop is topology-aware, meaning that it knows how to distribute jobs and tasks across nodes and across racks and data centers. That's fine for physical environments, but doesnt necessarily translate well to a virtualized environment.

Some challenges with enterprise deployments of Hadoop include:

* Slow to provision, complex to keep running

* Single points of failure with name node and job tracker

* No HA for Hadoop Framework Components (such as Hive, HCatalog, etc.)

* No easy way to share resources between Hadoop and non-Hadoop workloads

* Lack of resource containment

* No multi-tenancy or security/performance isolation functionality

* Lack of configuration isolation (can't run multiple versions on the cluster)

Virtualization presents itself as a potential solution to help address these enterprise Hadoop challenges.

This leads to a discussion of Project Serengeti, which (in its first release) helps automate the process of provisioning a virtual Hadoop cluster in just a matter of minutes. Serengeti deploys as a single vApp. After logging into the vApp (the CLI is similar to Hadoop), issue the "cluster create" command, and Serengeti will go deploy a fully-functional 3 node Hadoop cluster. Serengeti is driven by a JSON file that specifies the distribution of Hadoop, the cluster size, the type of storage (shared or local), etc. Serengeti supports a number of commercial and open source projects around Hadoop.

Hadoop relies heavily on the storage subsystem. Although local storage is extraordinarily cheap and unreliable, but when used in conjunction with something like HDFS it does look quite attractive. In vSphere environments, though, the most common form of storage is SAN/NAS. How does this work when you bring Hadoop into the mix? Using a hybrid model that leverages both shared and local storage. Hadoop seems to generate the majority of the disk I/O for temporary storage; you can use the hybrid model to store reliable data on SAN/NAS but the working temporary data on local storage. Recall that HDFS protects the data by creating multiple copies of the data, so failure of a local host (and it's associated storage) isn't that big of a deal.

Four common performance concerns that come up with running Hadoop on vSphere:

1. How well does Hadoop run virtualized?

2. Performance implications of running Hadoop on shared storage?

3. What's the effect of protecting Hadoop master daemons using FT?

4. Should we rent (public) or build (private) for running Hadoop?

Quick comparisons of Hadoop on native hardware versus Hadoop virtualized shows a small decrease in performance with only a single VM, but when the workload is divided into 2 or 4 VMs then performance returns closer to native performance. This was especially true with the TeraSort workload VMware used in their testing. With regard to storage, local storage was faster in all instances, with the exception of the TeraValidate workload working against a JBOD configuration on the SAN. There were significant performance impacts when SAN RAID overhead was introduced. This was especially true for the TeraGen workload, which was a write-heavy workload.

For FT usage (question #3 above), the testing showed a 2-4% slowdown for TeraSort when FT was enabled for the name node and job tracker nodes.

The cost comparison showed a vSphere implementation that was significantly smaller in size, but only 5x more time required to run TeraSort, and able to do it at 1/3 the cost.

The presentation now shifts to a discussion of some joint development work between VMware and Hortonworks, specifically around efforts to provide HA (high availability) for Hadoop components. If the name node fails, the entire job fails and has to be restarted; thus, the name node is typically protected with great effort. Would vSphere HA be enough to protect the name node? Normally jobs would fail. What VMware and Hortonworks have done is add some Hadoop-specific awareness (a JVM monitor to serve as a watchdog for the name node services) as well as add some pause/retry functionality for Hadoop-enabled applications. The Hortonworks HDP 1.0 release has these patches and the patches have been pushed upstream (to Apache, I believe). This means that name nodes (or job trackers) that you want to protect with HA need to be on shared storage, but worker nodes (which can be physical or virtual) will use local storage.

The third and final topic for Hadoop on vSphere involves elastic scaling. The solution that VMware came upon was to separate the job node and the data node into separate VMs; this enabled multi-tenancy as well as fixing some of the "content isolation" and "configuration isolation" issues mentioned earlier as well.

At this point, the session wrapped up.
