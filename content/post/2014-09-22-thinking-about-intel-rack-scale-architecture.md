---
author: slowe
categories: Musing
comments: true
date: 2014-09-22T09:00:00Z
slug: thinking-about-intel-rack-scale-architecture
tags:
- Hardware
- IDF2014
- Intel
title: Thinking About Intel Rack-Scale Architecture
url: /2014/09/22/thinking-about-intel-rack-scale-architecture/
wordpress_id: 3536
---

You may have heard of Intel Rack-Scale Architecture (RSA), a new approach to designing data center hardware. This is an idea that was discussed extensively a couple of weeks ago at Intel Developer Forum (IDF) 2014 in San Francisco, which I had the opportunity to attend. (Disclaimer: Intel paid my travel and hotel expenses to attend IDF.)

Of course, IDF 2014 wasn't the first time I'd heard of Intel RSA; it was [also discussed last year][1]. However, this year I had the chance to really dig into what Intel is trying to accomplish through Intel RSA---note that I'll use "Intel RSA" instead of just "RSA" to avoid any confusion with the security company---and I wanted to share some of my thoughts and conclusions here.

Intel always seems to present Intel RSA as a single entity that is made up of a number of other technologies/efforts; specifically, Intel RSA is typically presented as:

* Disaggregation of the compute, memory, and storage capacity in a rack

* Silicon photonics as a low-latency, high-speed rack-scale fabric

* Some software that combines disaggregated hardware capacity over a rack-scale fabric to create "pooled systems"

When you look at Intel RSA this way---and this is the way that it is typically positioned and described---it just doesn't seem to make sense. What's the benefit to consumers? Why should they buy this architecture instead of just buying servers? The Intel folks will talk about right-sizing servers to the workload, but that's not really a valid argument for the vast majority of workloads that are running virtualized on a hypervisor (or containerized using some form of container technologies). Greg Ferro (of [Packet Pushers](http://packetpushers.net/) fame) was also at the show, and we both shared some of these same concerns over Intel RSA.

Determined to dig a bit deeper into this, I went down to the Intel RSA booth on the IDF show floor. The Intel guys were kind enough to spend quite a bit of time with me talking in-depth about Intel RSA. What I came away with from that discussion is that Intel RSA is really about three _related but not interdependent_ initiatives:

* Disaggregating server hardware

* Establishing hardware-level APIs

* Enabling greater software intelligence and flexibility

I think the "related but not interdependent" phrase is really important here. What Intel's proposing with Intel RSA doesn't _require_ disaggregated hardware (i.e., shelves of CPUs, memory, and storage); it could be done with standard 1U/2U rack-mount servers. Intel RSA doesn't _require_ silicon photonics; it could leverage any number of low-latency high-speed fabrics (including PCI Express and 40G/100G Ethernet). Hardware disaggregation and silicon photonics are only potential options for how you might assemble the hardware layer.

The real key to Intel RSA, in my opinion, are the hardware-level APIs. Intel has team up with a couple vendors already to create a draft [hardware API specification called Redfish](http://www.redfishspecification.org). If Intel is successful in building a standard hardware-level API that is consistent across OEM platforms, then the software running on that hardware (which could be traditional operating systems, hypervisors, or container hosts) can leverage greater visibility into what's happening in the hardware. This greater visibility, in turn, allows the software to be more intelligent about how, where, and when workloads (VMs, containers, standard processes/threads) get scheduled onto that hardware. You can imagine the sorts of enhanced scheduling that becomes possible as hypervisors, for example, become more aware and more informed about what's happening inside the hardware.

Similarly, if Intel is successful in establishing hardware-level APIs that are consistent across OEM platforms, then the idea of assembling a "composable pooled system" from hardware resources---in the form of disaggregated hardware or in the form of resources spread across traditional 1U/2U servers and connected via a low-latency high-speed fabric---now becomes possible as well. This is why I say that the hardware disaggregation piece is only an _optional_ part of Intel RSA: with the widespread adoption of standardized hardware-level APIs, the same thing can be achieved without hardware disaggregation.

In my mind, Intel RSA is more of an example use case than a technology in and of itself. The real technologies here---disaggregated hardware, hardware-level APIs, and high-speed fabrics---can be leveraged/assembled in a variety of ways. Building rack-scale architectures using these technologies is just one use case.

I hope that this helps explain Intel RSA in a way that is a bit more consumable and understandable. If you have any questions, corrections, or clarifications, please feel free to speak up in the comments.

[1]: {{< relref "2013-09-11-idf-2013-rack-scale-architecture-for-cloud.md" >}}
