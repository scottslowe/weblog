---
author: slowe
categories: Explanation
comments: true
date: 2010-12-20T09:00:00Z
excerpt: This post is a follow-up to a couple of previous posts on VPLEX. In this
  post, I'll build on the information in those earlier posts to show how it is possible
  to use VPLEX in conjunction with a data replication solution.
slug: using-vplex-and-data-replication-together
tags:
- EMC
- Storage
- VPLEX
title: Using VPLEX and Data Replication Together
url: /2010/12/20/using-vplex-and-data-replication-together/
wordpress_id: 2188
---

In my previous post on EMC VPLEX, I provided [a review of the VPLEX storage objects][1]. In that post, I discussed the difference between a storage volume, an extent, a device, and a virtual volume. If you are unclear on the distinctions between those terms, I suggest you go back and read that post again before proceeding.

The following diagram illustrates these various storage objects again:

![VPLEX storage objects](/public/img/vplex-devices-extents.png)

As you can see from the diagram, VPLEX offers a number of different ways to combine these objects together:

* You can slice a single storage volume into multiple extents, then use each of those extents to create a separate device (and then a virtual volume). The VPLEX device might only occupy one of the extents on the storage volume and other data might occupy other extents on the storage volume. This is the option on the left side of the figure.

* You can combine extents from different storage volumes together into a single device (and then a virtual volume). This is the option on the right side of the figure.

* Finally, you can create a single extent occupying all of a storage volume and use it to create a single device. This is the middle option illustrated above and, as you'll see shortly, is the generally recommended way of doing it.

Now that I've reviewed the storage concepts again, let me delve into the real meat of this post. Since the introduction of EMC VPLEX and its storage federation functionality (which apparently I'm not supposed to call storage federation; see the comments [here][2]), organizations have another choice in their disaster avoidance/disaster recovery (DA/DR) plans. In addition to utilizing data replication solutions, organizations now have the option of integrating VPLEX's data synchronization functionality into their DA/DR solution. Is there room for VPLEX in an organization's DA/DR planning? The answer is yes! While there has been a great deal of confusion about how, if at all, VPLEX could be used _in conjunction with_ (rather than instead of) existing replication solutions, it is possible and it is fully supported (with a few caveats). This post discusses how you can use both VPLEX and data replication in the same environment.

As a side note, a fair amount of the information in this post is derived from a white paper, published by EMC, on the use of [array-based replication with EMC VPLEX](http://www.emc.com/collateral/hardware/white-papers/h8005-array-based-replication-vplex-wp.pdf). I do encourage you to read that white paper for more information and more details.

So what would a supported configuration that uses both VPLEX and replication look like? This diagram graphically depicts a supported solution that integrates both EMC VPLEX and a data replication solution such as SRDF or RecoverPoint:

![VPLEX replication topology](/public/img/vplex-repl-topol.png)

The key to a supported solution that combines VPLEX and replication lies in the phrase "single extent/single device". In order to use both replication as well as VPLEX in the same solution, **you must use a 1:1:1 mapping between storage volumes, extents, and devices.** In other words, you must use a "single extent/single device" approach, where each storage volume has only a single extent (occupying the entire storage volume), and devices are built from that single extent on a single storage volume.

If you think about it for a minute, you can easily see why this is the case. Let's consider a scenario in which you are wishing to combine VPLEX with RecoverPoint:

* What is the smallest unit of replication for RecoverPoint? A single LUN. Sure, you can replicate multiple LUNs or groups of LUNs, but the smallest unit of replication is a single LUN. You can't replicate part of a LUN.

* What is the smallest unit of federation for VPLEX? An extent, which might be part of a back-end storage volume (a LUN). You can federate multiple extents or groups of extents, but you can't federate a part of an extent.

* How do we get these two units lined up with each other? If RecoverPoint works only on entire LUNs and VPLEX works on extents, then the way to line them up is to ensure that each extent represents an entire LUN. This makes RecoverPoint's basic unit (an entire LUN) the same as VPLEX's basic unit (an entire LUN).

With your replication solution replicating entire LUNs and VPLEX working on entire LUNs, this creates the sort of situation where I/O can pass through VPLEX to the back-end array, where the replication solution can pick it up and replicate it off to a third location. This gives organizations the best of both worlds---support for disaster avoidance using VPLEX at synchronous distances and support for disaster recovery using a replication solution such as RecoverPoint or SRDF at longer distances.

Your feedback is always welcome! Post any questions, corrections, or clarifications in the comments below.

[1]: {{< relref "2010-12-13-a-quick-review-of-vplex-storage-objects.md" >}}
[2]: {{< relref "2010-06-07-a-deeper-look-at-vplex.md" >}}
