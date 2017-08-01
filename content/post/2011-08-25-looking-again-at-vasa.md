---
author: slowe
categories: Explanation
comments: true
date: 2011-08-25T09:00:00Z
slug: looking-again-at-vasa
tags:
- Storage
- Virtualization
- VMware
- vSphere
title: Looking Again at VASA
url: /2011/08/25/looking-again-at-vasa/
wordpress_id: 2373
---

In early August, I wrote a post titled [A Deeper Look at VASA][1], in which I provided some additional details on how VASA (the vSphere Storage APIs for Storage Awareness) worked and some of the potential limitations of the initial VASA implementation in vSphere 5. My original post was not intended to slam VASA; rather, my goal was to provide some additional information about how VASA works so that users can appropriately understand how best to incorporate this functionality into their environments.

Since that article was published, Cormac Hogan of VMware has published an article on the VMware vSphere blog that contains [a "sneak peek" at some of the VASA implementations](http://blogs.vmware.com/vsphere/2011/08/a-sneak-peek-at-how-vmwares-storage-partners-are-using-vasa.html) from various storage vendors. Now that we've had the ability to see how the various storage vendors are implementing VASA, I'd like to go back and look at the information about VASA in light of this new information.

In my first VASA post, I explained that the VASA protocol specification allows the storage provider to pass up only a single system-provided capability, and I outlined three different options for how storage vendors might use this functionality. Based on the information in Cormac's post (which I _highly recommend_ you read), it would appear that a couple of the storage vendors have chosen to embed multiple attributes (RAID, disk type, snapshots, etc.) in the name of the system-provided capability. For example, Cormac's article shows that Dell EqualLogic will use system-provided capabilities like "RAID, SNAP" or "RAID, SNAP, REPLICATED". This is a perfectly acceptable use of the system-provided capability, as I outlined in [my previous article][1]. The only drawback---and this isn't really a drawback as much as an observation---is that the overall list of potential system-provided capabilities can grow very long as more individual attributes are added to the name. 

Similarly, NetApp has chosen to include specific storage attributes in the name of the system-provided capabilities, creating capabilities like "Fiber Channel Disks; Dedupe" and "SATA Disks; Dedupe; Replication". Again, a perfectly acceptable workaround for the "one system-provided capability per datastore" limitation found in the VASA protocol specification.

Based on Cormac's article, it sounds like HP's VASA implementation will be very similar.

What about EMC's VASA implementation? I know what it will look like, but I haven't gotten approval to share that with anyone publicly yet, so I'll have to defer on any comments regarding EMC's VASA implementation. EMC does have a working VASA implementation and will provide VASA support, as outlined in Cormac's article.

So, based on this information and seeing how the vendors are "bypassing" the protocol limitation by overloading the system-provided capability name string, it would seem to me that there isn't really a strong limitation here. Personally, I would still recommend to VMware to stop using the term "capability" and find some other term, as I think that it leads users to believe VASA is something other than what it is (something more granular). I also think that VASA has a lot of room to grow and become more powerful in future releases, and I'm looking forward to seeing how this functionality evolves.

What do you think? Do these VASA implementations look useful? What part of VASA (and profile-driven storage) do you think will be most useful to you in your specific environment? I'd love to hear what you think, so feel free to speak up in the comments below.

[1]: {{< relref "2011-08-12-a-deeper-look-at-vasa.md" >}}
