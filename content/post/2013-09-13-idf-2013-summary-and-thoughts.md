---
author: slowe
categories: Information
comments: true
date: 2013-09-13T11:40:26Z
slug: idf-2013-summary-and-thoughts
tags:
- Hardware
- IDF2013
- Networking
title: IDF 2013 Summary and Thoughts
url: /2013/09/13/idf-2013-summary-and-thoughts/
wordpress_id: 3297
---

I'm back home in Denver after spending a few days in San Francisco at Intel Developer Forum (IDF) 2013, so I thought I'd take a few minutes to sit down and share a summary of the event and my thoughts.

First, here are links to all the liveblogging I did while at the conference:

[IDF 2013 Keynote, Day 1][1]  

[Enhancing OpenStack with Intel Technologies for Public, Private, and Hybrid Cloud][2]  

[IDF 2013 Keynote, Day 2][3]  

[Rack Scale Architecture for Cloud][4]  

[Virtualizing the Network to Enable a Software-Defined Infrastructure (SDI)][5]  

[The Future of Software-Defined Networking with the Intel Open Network Platform Switch Reference Design][6]  

[Enabling Network Function Virtualization and Software Defined Networking with the Intel Open Network Platform Server Reference Architecture Design][7]  

Overall, I enjoyed the event and found it quite useful. It appears to me that Intel has a three-prong strategy:

1. Expand the footprint of IA (Intel Architecture, what everyone else calls x86, x86_64, or x64) CPUs by moving into adjacent markets

2. Extend Intel's reach with non-IA hardware (FM6000 series, QuickAssist Server Acceleration Card [QASAC]

3. Use software, especially open source software, to drive more development toward Intel-based solutions

I'm sure there's probably more, but those are the ones that really stand out. You can see some evidence of these moves:

* Intel's Open Network Platform (ONP) Switch reference design (aka "Seacliff Trail") helps drive #1 and #2; it contains an IA CPU for programmability and leverages the FM6000 series for high-speed networking functionality

* Intel's ONP Server reference design ("Sunrise Trail") pushes Intel-based servers into markets they haven't traditionally seen (telco/networking roles), especially in conjunction with strategy #3 above (as shown by the strong use of Intel DPDK to optimize Sunrise Trail for SDN/NFV applications)

* Intel's Avoton and newly-announced Quark families push Intel into new markets (micro-servers, tablets, phones, sensors) where they haven't traditionally been a major player

All in all, it will be very interesting to see how things play out. As others have said, an definitely an interesting time to be in technology.

As with other relevant industry conferences (like VMworld, for example), one of the values of IDF is engaging in conversations with other professionals. I had a few of these conversations while at IDF:

* I spent some time talking with an Intel employee about Intel's Cache Acceleration Software (CAS), which came out of the acquisition of a company called Nevex. I wasn't even aware that Intel was doing cache acceleration. Intel CAS operates at the operating system (OS) level, serving as a file-level cache on Windows and a block-level cache (with file awareness) on Linux. It also supports caching to a remote SSD (in a SAN, for example) so that you can still use vMotion in vSphere environments. In the near future, they're looking at supporting cache clustering with a cache coherence algorithm that would allow you to use SSDs/flash from multiple servers as a single cache.

* I had a brief conversation with an Intel engineer who specialized in SSDs (didn't get to hit him up for some free Intel DC S3700s, though). We touched on a number of different areas, but one interesting statistic that came out of the conversation was the reality behind the "running out of writes" on an SSD. (This refers to the number of times you can write data to an SSD, which is made out to be a pretty big deal by some folks.) He spoke of a test that wrote 45GB an hour to an SSD; even at that rate, would have taken multiple decades of use before the SSD would not have been able to perform any writes.

* Finally, I spent some time chatting with my friend Brian Johnson, who works in the networking group at Intel. There's lots of cool stuff going on there, but---unfortunately---I can't really discuss most of what he shared with me. Sorry folks! We did have an interesting discussion around the user experience, personal data, mobility, and ubiquitous connectivity. Who knows---maybe the next great startup will emerge out of our discussion! :-)

Anyway, that's it for my IDF 2013 coverage. I hope that some of the information I shared proves useful to you in some way. As usual, courteous comments (with vendor disclosures, where needful) are always welcome.

_(Disclosure: I work for VMware, but was invited to attend IDF at Intel's expense.)_

[1]: {{< relref "2013-09-10-idf-2013-keynote-day-1.md" >}}
[2]: {{< relref "2013-09-10-idf-2013-enhancing-openstack-with-intel-technologies.md" >}}
[3]: {{< relref "2013-09-11-idf-2013-keynote-day-2.md" >}}
[4]: {{< relref "2013-09-11-idf-2013-rack-scale-architecture-for-cloud.md" >}}
[5]: {{< relref "2013-09-11-idf-2013-virtualizing-the-network-to-enable-sdi.md" >}}
[6]: {{< relref "2013-09-12-idf-2013-future-of-sdn-with-the-intel-onp-switch-reference-design.md" >}}
[7]: {{< relref "2013-09-12-idf-2013-enabling-nfvsdn-with-intel-onp-server-reference-design.md" >}}
