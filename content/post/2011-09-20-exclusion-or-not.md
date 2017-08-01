---
author: slowe
categories: Rant
comments: true
date: 2011-09-20T17:39:16Z
slug: exclusion-or-not
tags:
- EMC
- HP
- NetApp
- Storage
- Virtualization
- VMware
- VMworld2011
- vSphere
title: Exclusion or Not?
url: /2011/09/20/exclusion-or-not/
wordpress_id: 2425
---

A couple days ago I read Stephen Foskett's article ["Alas, VMware, Whither HDS?"](http://blog.fosketts.net/2011/09/18/vmware-vaai-hds/), and I felt like I really needed to respond to this growing belief---stated in Stephen's article and in the sources to his article---that VMware is, for whatever reason, somehow excluding certain storage vendors from future virtualization-storage integration development. From my perspective, this is just bogus.

As far as I can tell, Stephen's post---which is just one of several I've seen on this subject---is based on two sources: [my session blog of VSP3205][1] and [an article by The Register](http://www.theregister.co.uk/2011/09/09/vmware_lun_war/). I wrote the session blog, I sat in the session, and I listened to the presenters. Never once did one of the presenters indicate that the five technology partners that participated in this particular demonstration were the _only_ technology partners with whom they would work moving forward, and my session blog certainly doesn't state---or even imply---that VMware will only work with a limited subset of storage vendors. In fact, the thought that other storage vendors would be excluded never even crossed my mind until the appearance of The Register's post. That invalidates my VSP3205 session blog as a credible source for the assertion that VMware would be working with only certain storage companies for this initiative.

The article at The Register cites my session blog and [a post by Wikibon analyst David Floyer](http://wikibon.org/wiki/v/VMware_Storage_Innovation) as a source. I've already shown how my blog doesn't support the claim that some vendors will be excluded, but what about the other source? The Wikibon article states this:

>Wikibon understands that VMware plans to work with the normal storage partners (Dell, EMC, Hewlett Packard, IBM, and NetApp) to provide APIs to help these traditional storage vendors add value, for example by optimizing the placement of storage on the disks.

This statement, however, is not an indication that VMware will work only with the listed storage vendors. (Floyer does not, by the way, cite any sources for that statement.)

Considering all this information, the _only_ place that is implying VMware will limit the storage vendors with whom they will work is Chris Mellor at The Register. However, even Chris' article quotes a VMware spokesperson who says:

>"Note that were still in early days on this and none of the partners above have yet committed to support the APIs  and while it is our intent to make the APIs open, currently that is not the case given that what was demod during this VMworld session is still preview technology."

In other words, just because HDS or any other vendor didn't participate (which might indicate that the vendor _chose_ not to participate) **does not** mean that they are somehow excluded from future inclusion in the development of this proposed new storage architecture. In fact, participation---or lack thereof---at this stage really means nothing, in my opinion. If this proposed storage architecture gets its feet under it and starts to run, then I'm confident VMware will allow any willing storage vendor to participate. In fact, it would be detrimental to VMware to _not_ allow any willing storage partner to participate.

However, it gets more attention if you proclaim that a particular storage vendor was excluded; hence, the title (and subtitle) that The Register used. I have a feeling the reality is probably quite different than the picture painted in some of these articles.

[1]: {{< relref "2011-08-29-vsp3205-tech-preview-vstorage-apis.md" >}}
