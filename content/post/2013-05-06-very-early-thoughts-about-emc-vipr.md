---
author: slowe
categories: Musing
comments: true
date: 2013-05-06T18:44:45Z
slug: very-early-thoughts-about-emc-vipr
tags:
- EMC
- Storage
- Virtualization
title: Very Early Thoughts about EMC ViPR
url: /2013/05/06/very-early-thoughts-about-emc-vipr/
wordpress_id: 3173
---

EMC announced ViPR today, the culmination of the not-so-secret Project Bourne and its lesser-known predecessor, Project Orion. Although I used to work at EMC before [I joined VMware earlier this year][1], I never really had deep access to what was going on with this project, so my thoughts here are strictly based on what's been publicly disclosed. Naturally, given that the product was only announced today, these are _very_ early thoughts.

Naturally, Chad Sakac has [a write-up on ViPR](http://virtualgeek.typepad.com/virtual_geek/2013/05/storage-virtualization-platform-re-imagined.html) and what led up to its creation. It's worth having a read, but allocate plenty of time (it _is_ a bit on the long side).

Based on the limited material that is publicly available so far, here are a few thoughts about ViPR:

* I like the control plane-data plane separation model that EMC is taking with ViPR. I've had a few conversations about network virtualization and software-defined networking (SDN) recently (see [here][2] and [here][3]) and the amorphous use of the term "software-defined." In fact, my good friend Matthew Leib wrote [a post about software-defined storage](http://virtuallytiedtomydesktop.wordpress.com/2013/05/03/software-defined-storage-what-does-that-mean-to-me-anyway/) in response to an exchange of tweets about the overuse of "software-defined _[insert whatever here]_". If we go back to the original definition of what SDN meant, it referred to the separation of the networking control plane from the networking data plane and the architectural changes resulting from that separation. SDN wasn't (and isn't) about virtualizing network switches, routers, or firewalls; that's NFV (Network Functions Virtualization). Similarly, running storage controller software as virtual machines isn't software-defined storage, it's the storage equivalent of NFV (SFV?). Separating the storage control plane from the storage data plane is a much closer storage analogy to SDN, in my opinion. I'm sure that EMC hopes that it will spark a renaissance in storage the way SDN has sparked a renaissance in networking (more on that below).

* I like that EMC is including a variety of object storage APIs, including Atmos, AWS S3, and OpenStack Swift, and that there is API support for OpenStack Cinder and OpenStack Glance as well. It would have been the wrong move not to support these APIs in ViPR---in my opinion, EMC won't get another opportunity like this to broaden their API and platform support.

* Obviously, a key difference between SDN and SDS _a la_ ViPR is **openness.** While EMC proclaims the openness of the solution based on broad API support, 3rd party back-end storage support, a public northbound API, and source code and examples for third-party southbound "plugins" for other platforms, the reality is that this separation of control plane and data plane is being driven by a vendor rather than as a result of collaboration between academic research and industry. The reason this distinction is important is that it's one thing for a networking vendor to build OpenFlow support into its switches when OpenFlow wasn't and isn't created/controlled by a competing vendor, but it's another thing for a storage vendor to build support into their products for a solution that belongs to EMC. Whether this really matters or not remains yet to be seen---it may be a non-issue. (Yes, I recognize the irony in the fact that I work for VMware, some of whose solutions might be similarly criticized with regard to openness.)

* Hey, where's the network virtualization support? ;-)

Anyway, those are my initial thoughts. Since I haven't had access to more detailed information on what it does/doesn't support or how it works, I reserve the right to revise these thoughts and impressions after I get more exposure to ViPR. In the meantime, feel free to add your own thoughts in the comments below. Courteous comments are always welcome (but do please add vendor affiliations where applicable)!

**UPDATE:** It was brought to my attention I misspelled Matthew Leib's last name; that has been corrected. My apologies Matt!

[1]: {{< relref "2013-01-25-new-year-new-challenges-new-opportunities.md" >}}
[2]: {{< relref "2013-04-16-network-overlays-vs-network-virtualization.md" >}}
[3]: {{< relref "2013-04-30-on-network-virtualization-and-sdn.md" >}}
