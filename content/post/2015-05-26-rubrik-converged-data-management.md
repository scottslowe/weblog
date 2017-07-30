---
author: slowe
categories: News
comments: true
date: 2015-05-26T11:45:20Z
tags:
- Storage
- Hardware
- Cloud
title: Rubrik and Converged Data Management
url: /2015/05/26/rubrik-converged-data-management/
---

Rubrik [today announced][link-2] a new Series B investment (of $41 million) and introduced their r300 Series Hybrid Cloud Appliance, powered by what they're touting as a "Converged Data Management" platform. Wow---that's a mouthful, isn't it? It sounds a bit like buzzword bingo, but after having spent a bit of time talking to Rubrik last week, there are some interesting (in my opinion) things going on here.

So what exactly is Rubrik doing? Here's the "TL;DR" for those of you that don't have the patience (or the time) for anything more in-depth: Rubrik is targeting the secondary storage and backup/recovery market with a solution that combines a distributed file system, a distributed metadata service, clustering, and a distributed task scheduler to provide a scale-out backup/recovery solution that also seamlessly integrates cloud storage platforms for long-term retention. The catch-phrase they're using is "Time Machine for cloud infrastructure" (I wonder how our good friends in Cupertino will react to the use of that phrase?).

Here's a bit more detail on the various components of the solution:

* Rubrik has its own distributed file system (imaginatively named the Rubrik Cloud-Scale File System) that was designed from scratch to store and manage versioned data. The file system is optimized for a hybrid flash/disk architecture (Rubrik writes everything to flash first), and is a fully resilient scale-out file system---data is automatically replicated as needed to ensure that the loss of a single node doesn't result in data loss.
* Working along side their distributed file system is a distributed metadata service, which provides a scale-out index of metadata across a cluster of Rubrik appliances. This might seem insignificant, but I think it's actually one of the more interesting parts of the solution. (Of course, I'm no storage expert.)
* I've mentioned already that Rubrik bills their system as a clustered, scale-out system, and they include mechanisms for simplifying the addition of cluster nodes (using Zeroconf multicast DNS).
* In addition to the distributed metadata service, the second really interesting part of the architecture (to me) is the distributed task framework. Everything in Rubrik---backing up data, indexing the data, updating the distributed metadata service, replicating data to protect against node failure, migrating data to/from back-end cloud storage platforms---is handled as a task, and these tasks are distributed and scheduled across the cluster. 

So why do I think the metadata and task framework are really interesting? Between the distributed metadata service (which allows any node in the cluster to access or update metadata) and the distributed task framework, this means that any node in a cluster of Rubrik appliances can back up a VM and update the index with the contents of that backup. Lots of companies like to talk about "scale out" architectures, but in this case the Rubrik team really is building an architecture that allows backup and recovery to scale out with the size of the cluster. Data ingestion, data indexing, data migration (to/from backend cloud storage platforms), data replication---all of these can happen on any node in the cluster (of course, the task framework has data locality awareness and schedules tasks accordingly).

While many folks will zero in on the use of cloud storage platforms (Rubrik supports S3 right now, with plans for other cloud storage platforms and providers in the future) as new and innovative, that part is actually kind of "meh" for me. Yes, it certainly addresses a pain point with regards to managing tapes and offsite tape storage, but the use of cloud storage platforms as a backend for an on-premises platform isn't all that new.

There are a few other interesting things happening as well---Rubrik is completely API-driven (their web-based UI is strictly an API client), so that's a nice architectural decision. Rubrik's distributed metadata service also enables a "Google-like" search experience to find files that have been backed up (and that you might want to restore), which is certainly an improvement over some of the backup/restore UIs I've seen in the past. "Instant Recovery", where Rubrik instantly boots a recovered VM _on the Rubrik system_ (it presents itself as an NFS datastore) is kind of nice; users can then use Storage vMotion to move the recovered VMDK over to their primary storage platform.

When I talked to Rubrik, they also made a big deal out of what they're calling "Live Storage," which is using the platform's zero-copy clones to be able to instantly mount any copy of any backed-up data from the Rubrik platform itself (via NFS). This is a nice feature, but I am personally skeptical that it will appeal to the developer crowd as much as they think/hope that it will. Nevertheless, it is an interesting feature, and it may prove more attractive and/or useful to developers than I suspect.

Cormac Hogan has also posted a bit about Rubrik; see [his latest post][link-1]. Duncan Epping also [talked about Rubrik][link-3] back in March, as did [Frank Denneman][link-4] and [Arjan Timmerman][link-5]. I encourage you to review their articles as well for more information.


[link-1]: http://cormachogan.com/2015/05/26/a-closer-look-at-rubrik/
[link-2]: http://www.rubrik.com/blog/press-release/rubrik-raises-41-million-series-b-led-by-greylock-partners-announces-general-availability-of-r300-series-hybrid-cloud-appliance/
[link-3]: http://www.yellow-bricks.com/2015/03/24/startup-intro-rubrik-backup-and-recovery-redefined/
[link-4]: http://frankdenneman.nl/2015/03/24/dont-backup-go-forward-with-rubrik/
[link-5]: http://www.vdicloud.nl/2015/04/21/rubrik-change-in-the-backup-world/