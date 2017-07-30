---
author: slowe
categories: Liveblog
comments: true
date: 2012-09-11T13:23:25Z
slug: spcs001-intel-next-generation-haswell-microarchitecture
tags:
- Hardware
- IDF2012
- Intel
title: 'SPCS001: Intel Next-Generation Haswell Microarchitecture'
url: /2012/09/11/spcs001-intel-next-generation-haswell-microarchitecture/
wordpress_id: 2841
---

This is session SPCS001, titled "Technology Insight: Intel Next Generation Microarchitecture Code Name Haswell". The speakers are Tom Piazza, Hong Jiang, Per Hammarlund, and Ronak Singhal, Sr.

Haswell is a "tock" as opposed to a "tick" (referring to Intel's "tick/tock" release cycle); this means it is a significant change at the platform and microarchitecture levels. It is a 22nm platform (Ivy Bridge was also 22nm). Haswell will retain key features from the Sandy Bridge/Ivy Bridge platform, like Hyper-Threading, Turbo Boost, and Ring Interconnect.

A key philosophy for the Haswell microarchitecture is the use of a "converged code"; that is, a single microarchitecture that scales from tablets all the way to servers in the datacenter. That might seem odd, but the power advantages present in Haswell are just as applicable to tablets and mobile devices as they are to servers running tens (hundreds?) of cores in the data center.

Major focus areas for Haswell include performance improvements (not only for existing "legacy" code but also for new code), modularity, and power innovations.

With regard to modularity, Haswell enables a variety of permutations of core count, cache size, and other variables. This enables more flexibility by Intel's OEMs in delivering Haswell to a variety of platforms.

In the area of power innovations, Haswell still uses the S0 and S3/4 (active and sleep, respectively) power states. Intel is working to reduce overall power usage in S0, and working to reduce power usage and resume time for S3/4. Haswell introduces S0ix, which is "active/idle" state, which provides dramatically reduced power usage and dramatically reduced resume time (from multiple seconds to hundreds of milliseconds).

As the presenters went into performance improvements, the presentation got extremely technical. While it probably made sense to a developer (the target audience at IDF), much of it did not make sense to me. I've included in the information below in a bulleted format for completeness.

* Increased buffer sizes to allow for greater parallelism to be discovered in code execution

* Enhancements in branch prediction

* The addition of two more operations per cycle (Nehalem/Sandy Bridge could do 6 operations per cycle; Haswell can perform 8 operations per cycle)

* Doubling of floating point operations per cycle through the addition of two Fused Multiply-Add (FMA) operations

* Reduced virtualization latencies (no additional details provided)

* A new gather instruction that allows the system to read multiple locations in memory in one operation

* Introduction of AVX2 instruction set to further improve integer performance and vectorization

* Improved performance in bit manipulation operations; this should have an impact on cipher/encryption/decryption operations

* Introduction of TSX (Transactional Synchronization Extensions) to help with creating software that has greater parallelism

The session next transitioned into some discussions specific to improvements in graphics performance and media performance. Improved modularity in the graphics core allows for more "scale-out" performance; this is responsible for the 2x reduction in power usage for matching graphics performance when using Haswell. (This was part of Perlmutter's keynote.) With regards to media performance, Haswell introduces hardware-based SVC (Scalable Video Coding) and several hardware codecs, including an MPEG codec. Haswell also improves the Video Quality Engine (VQE), which supports an extensive suite of video processing functions. The end result is higher video quality at lower power usage.

The remainder of the session focused on specific power improvements in the media and graphics space, then closes with a summary of the improvements in the Haswell microarchitecture.
