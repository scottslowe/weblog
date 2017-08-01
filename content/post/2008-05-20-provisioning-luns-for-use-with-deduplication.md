---
author: slowe
categories: Information
comments: true
date: 2008-05-20T22:10:14Z
slug: provisioning-luns-for-use-with-deduplication
tags:
- NetApp
- ONTAP
- SAN
- Storage
title: Provisioning LUNs for Use with Deduplication
url: /2008/05/20/provisioning-luns-for-use-with-deduplication/
wordpress_id: 711
---

The recent couple of articles I wrote about using NetApp deduplication---in particular, the article on using [NetApp deduplication with block storage][1]---have raised some questions that are probably worth addressing. Although NetApp deduplication works just fine with block-based storage, there are some considerations with regards to how the LUNs should be provisioned when deduplication will come into play.

Fortunately for me, someone at NetApp decided that it would be a good idea to document the five basic configurations of using NetApp deduplication with block storage. As seen in a comment to [my earlier article][1], Larry Freeman points out [this document](http://communities.netapp.com/docs/DOC-1192;jsessionid=7177122E64AA32F30DD92CEE995AC70E) on the NetApp Communities site (has anyone else noticed the similarity between VMware Communities and NetApp Communities?) that outlines the 5 basic configurations and where the freed blocks go in each configuration. Excellent---that saves me some work!

The most common configurations I've seen are configurations D (LUN not space reserved, space guarantee set to Volume) and E (LUN not space reserved, space guarantee set to None). Customers like to see the LUNs "shrink" after deduplication, and this is the only way to make that happen.

The only things we need now are for NetApp to a) remove the volume size limitations and b) get us deduplication at the aggregate level. Then we'd really be set!

[1]: {{< relref "2008-04-24-using-netapp-deduplication-with-block-storage.md" >}}
