---
author: slowe
categories: Liveblog
comments: true
date: 2017-11-28T17:00:00Z
tags:
- AWS
- Storage
- reInvent2017
title: "Liveblog: Deep Dive on Amazon Elastic File System"
url: /2017/11/28/deep-dive-amazon-elastic-file-system/
---

This is a liveblog of the AWS re:Invent 2017 session titled "Deep Dive on Amazon Elastic File System (EFS)." The presenters are Edward Naim and Darryl Osborne, both with AWS. This is my last session of day 2 of re:Invent; thus far, most of my time has been spent in hands-on workshops with only a few breakout sessions today. EFS is a topic I've watched, but haven't had time to really dig into, so I'm looking forward to this session.<!--more-->

Naim kicks off the session with looking at the four phases users go through when they are choosing/adopting a storage solution:

1. Choosing the right storage solution
2. Testing and optimizing
3. Ingest (loading data)
4. Running it (operating it in production)

Starting with Phase 1, Naim outlines the three main things that people think about. The first item is storage type. The second is features and performance, and the third item is economics (how much does it cost). Diving into each of these items in a bit more detail, Naim talks about file storage, block storage, and object storage, and the characteristics of each of these approaches. Having covered these approaches, Naim returns to file storage (naturally) and talks about why file storage is popular:

* Works natively with operating systems
* Provides shared access while providing consistency guarantees and locking functionality
* Provides a hierarchical namespace

Generally speaker, file storage hits the "sweet spot" between latency and throughput compared to block and object storage.

According to Naim, the key features of EFS are:

* Simple (easy to use, operate, consume)
* Elastic (no problem growing to accommodate capacity)
* Scalable (consistent low latencies, thousands of concurrent connnections)
* Highly available and durable (all file system objects stored in multiple AZs)

Next, Naim shows the typical "customer logo" slide to show how widely adopted EFS is by the AWs customer base and some of the use cases seen. Although the presenter said he wasn't going to go through all the logos, he spends more than a few minutes going through almost all of them.

With regards to security, EFS offers a number of security-related features. Network access to EFS is controlled via security groups and NACLs. File and directory access is controlled via POSIX permissions; administrative access is managed via IAM. Encryption is also supported, with key storage in KMS.

Naim next goes through some pricing comparisons showing how EFS is much cheaper than DIY storage solutions using EC2 instances and EBS volumes.

EFS completes the "trifecta" of storage solutions that AWS offers, which cover the whole range of storage types (EFS for file, EBS for block, S3 for object).

A new feature announced last week is EFS File Sync, which is designed to get data from on-premises file systems into EFS, and is designed to operate up to 5x faster than traditional Linux copy tool. Naim indicates that Osborne will discuss this in more detail later in the session.

Next Naim discusses some architectural aspects of EFS, and how NFS clients on EC2 instances across various AZs might access EFS. EFS is POSIX-compliant and supports NFS v4.0 and v4.1. Of course, Naim points out that you should _always_ test to verify that everything works as expected.

Now Osborne steps up to talk specifically about performance. When creating an EFS instance, you can select either "general purpose" (the default) or "max I/O" (designed for very large scale-out workloads, at the cost of slightly higher latencies). GP may have lower latencies, but has a ceiling of 7K operations/second. For GP file systems, AWS does expose a CloudWatch metric that allows users to see where they fall within the 7K ops/sec limit.

Osborne next compares EFS and EBS PIOPS (not sure what this stands for). Again, there are trade-offs (there are always trade-offs with technology decisions). As with any file system, throughput is a function of I/O size, meaning that larger I/O size will create more throughput. To really take advantage of EFS, Osborne indicates that parallelism is the key.

Changing gears slightly, Osborne reviews the mount options for using EFS with Linux instances on EC2. Linux kernel 4.0 or higher is recommended, as is NFS v4.1. More details are available in the documentation, according to Osborne.

Overall throughput is gated/tied to file system capacity; sustained throughput is 50 MBps per TB of storage, with bursts up to 100 MBps.

Osborne shifts focus now to talk about ingest, i.e., getting data into EFS. He reviews a couple different options (connecting on-premises servers to EFS via Direct Connect or using a third-party VPN solution). Both of these options can, according to Osborne, be used not only for migration but also for bursting or backup/disaster recovery. In order to optimize data copy/ingest, parallelism is again the key. Osborne reviews a few standard Linux copy tools (like `rsync`, `cp`, `fpsync`, or `mcp`). According to Osborne, `rysnc` offers relatively poor performance. `fpsync` is essentially multi-threaded `rsync`; `mcp` is a drop-in replacement for `cp` developed by NASA; both of these tools offer better performance than their single-threaded counterparts. The best performance comes from combining tools with GNU Parallel.

This dicussion of ingest performance and tools brings Osborne back to the topic of EFS File Sync, which is a multi-threaded tool that uses parallelism to maximize throughput, and offers encrypted transfers to EFS for security. EFS File Sync can be used to transfer from on-premises to EFS, between EFS instances in different regions, or from DIY shared storage solutions to EFS.

Next, Osborne shows a recorded video of using EFS File Sync to copy data between two different EFS instances. The demo shows copying roughly 20GB of data in about 4 minutes.

Naturally, you can move objects from Amazon S3 into EFS; this would involve using an EC2 instance that accesses S3 (perhaps via the AWS CLI) and an NFS mount backed by EFS. Osborne recommends maximizing parallelism to get the best possible ingest performance. GNU Parallel comes up here again.

Naim steps up to take over to talk about operations. EFS exposes a number of CloudWatch metrics, and all EFS API calls can be logged to CloudTrail.

Osborne comes back up to talk about some reference architectures using EFS. The first example is a highly-available WordPress architecture, followed by similar architectures for Drupal and Magento. Another example architecture is an EFS backup solution implemented via a CloudFormation template.

Wrapping up the session, Naim reviews some additional resources available via the Amazon web site, and announces a new series of storage-focused training classes that provide more in-depth training on S3, EFS, and EBS. At this point, Naim closes out the session.
