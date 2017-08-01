---
author: slowe
categories: News
comments: true
date: 2012-03-18T16:14:18Z
slug: sr-iov-support-in-the-next-version-of-hyper-v
tags:
- Hardware
- HyperV
- Microsoft
- Networking
- Virtualization
title: SR-IOV Support in the Next Version of Hyper-V
url: /2012/03/18/sr-iov-support-in-the-next-version-of-hyper-v/
wordpress_id: 2558
---

While browsing my list of RSS feeds tonight, I came across a series of articles by John Howard, a senior program manager on the Hyper-V team at Microsoft. The post was one of a series of posts describing SR-IOV support in the next version of Hyper-V, found in Windows "8". I hadn't heard that Microsoft was adding SR-IOV support to the next version of Hyper-V, so when I saw that I was surprised. Personally, I think SR-IOV support is a big deal (see the note at the end of this post for why).

If you're not familiar with SR-IOV, I suggest you read [this quick SR-IOV tutorial][1] I published on this site in late 2009.

Here are the links to John's SR-IOV in Hyper-V posts:

[Everything you wanted to know about SR-IOV in Hyper-V, part 1](http://blogs.technet.com/b/jhoward/archive/2012/03/12/everything-you-wanted-to-know-about-sr-iov-in-hyper-v-part-1.aspx)  
[Everything you wanted to know about SR-IOV in Hyper-V, part 2](http://blogs.technet.com/b/jhoward/archive/2012/03/13/everything-you-wanted-to-know-about-sr-iov-in-hyper-v-part-2.aspx)  
[Everything you wanted to know about SR-IOV in Hyper-V, part 3](http://blogs.technet.com/b/jhoward/archive/2012/03/14/everything-you-wanted-to-know-about-sr-iov-in-hyper-v-part-3.aspx)  
[Everything you wanted to know about SR-IOV in Hyper-V, part 4](http://blogs.technet.com/b/jhoward/archive/2012/03/15/everything-you-wanted-to-know-about-sr-iov-in-hyper-v-part-4.aspx)  
[Everything you wanted to know about SR-IOV in Hyper-V, part 5](http://blogs.technet.com/b/jhoward/archive/2012/03/16/everything-you-wanted-to-know-about-sr-iov-in-hyper-v-part-5.aspx)

It's great to see Microsoft adding SR-IOV support to Hyper-V; this brings SR-IOV out of the niche Linux market and into a broader, more mainstream market. This also applies some competitive pressure against market leader VMware, who now has to respond in some fashion---either by adding SR-IOV support to their ESXi hypervisor, or by explaining why SR-IOV support isn't necessary. Personally, I hope that VMware does the former and not the latter.

(By the way, for those of you wondering why SR-IOV is important, there are lots of potential synergies here---in my view, at least---between hardware switching on an SR-IOV NIC and things like software-defined networking.)

[1]: {{< relref "2009-12-02-what-is-sr-iov.md" >}}
