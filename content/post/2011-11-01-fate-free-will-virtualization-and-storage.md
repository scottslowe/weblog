---
author: slowe
categories: Musing
comments: true
date: 2011-11-01T09:11:48Z
slug: fate-free-will-virtualization-and-storage
tags:
- EMC
- Storage
- Virtualization
- VMware
title: Fate, Free Will, Virtualization, and Storage
url: /2011/11/01/fate-free-will-virtualization-and-storage/
wordpress_id: 2452
---

You might wonder what fate and free will have to do with virtualization and storage. The title of this post is a reference to the debate of Fate vs. Free Will, which in turn is a reference to Stephen Foskett's recent post [VMware as Oedipus: How Server Virtualization will Change Storage Forever](http://blog.fosketts.net/2011/10/31/vmware-oedipus-server-virtualization-change-storage/). I won't provide all the details here (go read the post), but the basic idea behind the post is that VMware's drive to add storage features to the virtualization stack puts it on a collision course with EMC, a leading storage vendor. The twist here is the fact that EMC has a majority ownership in VMware, thereby earning EMC the term "parent company" and creating the Oedipal conflict to which Stephen alludes in his post.

First, let me sum up Stephen's points:

1. VMware is causing users _not_ to purchase storage arrays.

2. VMware integration is "leveling" the playing field.

Let's take a look at each of these points.

## A Decrease in Shared Storage?

Stephen makes this statement in his article (emphasis his):

>VMware is rapidly innovating in this area. Integrating and developing snapshot, replication, thin provisioning, and other features in VMFS enables everyone to have advanced storage functionality, regardless of which storage device they use. **In this way, VMware is already causing many users to forego an enterprise storage array purchase.**

Perhaps it's the specific term "enterprise storage array," but I have a hard time believing that the adoption of VMware is causing users to forgo array purchases. Think about it: to even use many of the advanced features of vSphere like vSphere HA, vSphere DRS, or vSphere FT, shared storage is a prerequisite. Users literally _cannot_ use these features without shared storage, and---today, at least---shared storage in almost all cases means an array.

If, however, the statement is intended to say that VMware users are buying less feature-rich arrays because of the features being added into vSphere---features like snapshots, replication, and thin provisioning---then I suppose I can see that. This is why array vendors are (or should be) driving innovation in other areas, such as dynamic auto-tiering, more robust snapshotting functionality, higher availability, and higher levels of performance.

Additionally, this is an opportunity for both virtualization experts and storage experts to help customers understand the differences between the features provided by the hypervisor and features provided at the storage layer. While these features share names, they can be very different! Here are a couple specific examples:

* VMware's snapshots are fundamentally and dramatically different from the snapshot features offered by many storage vendors. Not only are they different in how they work, they are also different in their uses and usage patterns. Storage administrators use array-based snapshots for different purposes and in different ways than vSphere administrators use snapshots.

* vSphere's replication functionality is a nice "check box" item, but lacks many of the features that array-based replication offers. For example, there's no compression, no deduplication or WAN optimization, and no idea of consistency groups.

As you can see, while it's true that VMware is offering features that are _similar_ in name and purpose, these features often are not true competitors to the features that storage array vendors offer, EMC included. Looking ahead, I anticipate that will continue to be the case, and storage vendors will continue to have ample opportunities to offer functionality above and beyond what the hypervisor can or will offer.

## Homogenization of Storage?

The second point of the article is the assertion that "ever tighter integration serves to anonymize and homogenize enterprise storage devices." I don't agree here, for a couple of reasons:

1. This statement assumes that all integration is the same, which is not the case. One vendor's level and type of integration with VMware can be very different than another vendor's level and type of integration. A vCenter plug-in is not the end of the story. What about integration with your replication solution? What about integration with your snapshot functionality? What about the quality of your VAAI implementation? One vendor's implementation of VAAI might behave quite differently than another vendor's implementation. What about your support of VMware's multipathing APIs? I could go on and on, but you get the idea.

2. This statement excludes the value of innovation in other areas, implying that VMware integration is the sole factor that levels the playing field. As I've stated on many occasions, every storage solutions has its advantages and disadvantages. The way that EMC does things gives it an advantage over NetApp in some areas; at the same time, the way that NetApp does things gives it an advantage over EMC in other areas. If all arrays were the same and were measured only on their VMware integration, then I could see this statement. That's not the case. And even _if it were_, then as I've just shown you, VMware integration can take many forms and many levels. Despite ever increasing levels of integration, vendors still have plenty of opportunities to differentiate themselves from other vendors through price, performance, data protection, scalability, reliability, and availability.

## Conclusion

I don't disagree that VMware will change the nature of enterprise storage; in fact, I would argue that _it already has_ changed enterprise storage. But to say that VMware will completely anonymize and homogenize enterprise storage is, in my humble opinion, a bit of a reach. There are still plenty of areas in which storage vendors can innovate and differentiate, both _in addition to_ as well as _in spite of_ VMware's own storage-related ambitions.

_Disclaimer: It's probably well-known anyway, but it's important to state that I do work for a storage vendor (EMC), although---as my site-wide disclaimer indicates---content here is not sponsored by, reviewed by, or even approved by my employer._
