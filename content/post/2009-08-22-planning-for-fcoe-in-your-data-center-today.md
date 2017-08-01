---
author: adelp
categories: Education
comments: true
date: 2009-08-22T15:08:05Z
excerpt: Here are some thoughts on planning for Fibre Channel over Ethernet (FCoE)
  in your data center.
slug: planning-for-fcoe-in-your-data-center-today
tags:
- Cisco
- FCoE
- FibreChannel
- Nexus
- NFS
- SAN
- Storage
- Virtualization
- VMware
- vSphere
- GuestPost
title: Planning for FCoE in Your Data Center Today
url: /2009/08/22/planning-for-fcoe-in-your-data-center-today/
wordpress_id: 1539
---

_By Aaron Delp_

This week I've had the privilege of attending a Cisco Nexus 5000/7000 class. I have learned a tremendous amount about FCoE this week and after some conversations with Scott about the topic, I wanted to tackle it one more time from a different point of view. I have included a list of some of Scott's FCoE articles at the bottom for those interested in a more in-depth analysis.

Disclaimer: I am by no means an FCoE expert! My working knowledge of FCoE is about four days old at this point. If I am incorrect in some of my situations below, please let me know (keep it nice and professional, people!) and I will be happy to make adjustments.

If you are an existing VMware customer today with FC as your storage transport layer, should you be thinking about FCoE? How would you get started? What investments can you make in the near future to prepare you for the next generation?

These are questions I am starting to get from my customers in planning/white board sessions around VMware vSphere and the next generation of virtualization. The upgrade to vSphere is starting to prompt planning and discussions around the storage infrastructure.

Before I tackle the design aspect, let me start out with some hardware and definitions.

**Cisco Nexus 5000 series switch:** The Nexus 5K is a Layer 2 switch that is capable of FCoE and can provide both Ethernet and FC ports (with an expansion module). In addition to Ethernet switching, the switch also operates as an FC fabric switch providing full fabric services, or it can be set for N_Port Virtualization (NPV) mode. The Nexus 5K can't be put in FC switch mode and NPV mode at the same time. You must pick one or the other.

**N_Port Virtualization (NPV) mode:** NPV allows the Nexus 5K to act as a FC "pass thru" or proxy. NPV is great for environments where the existing fabric is _not_ Cisco and merging the fabrics could be ugly. There is a downside to this. In NPV mode, no targets (storage arrays) can be hung off the Nexus 5K. This is true for both FC and FCoE targets.

**Converged Network Adapter (CNA):** A CNA is single PCI card that contains both FC and Ethernet logic, negating the need for separate cards, separate switches, etc.

Now that the definitions and terminology is out of the way, I see four possible paths if you have FC in your environment today.

**_1. FCoE with a Nexus 5000 in a non-Cisco MDS environment (merging)_**

In this scenario, the easiest way to get the Nexus on the existing non-Cisco FC fabric is to put the switch in NPV mode. You could put the switch in interop mode (and all the existing FC switches), but it is a nightmare to get them all talking and you often lose vendor specific features in interop mode. Plus, to configure interop mode, the entire fabric has to be brought down. (You _do_ have redundant fabrics, right?)

With the Nexus in NPV mode, what will it do?  Not much. You can't hang storage off of it. You aren't taking advantage of Cisco VSANs or any other features that Cisco can provide. You are merely a pass thru. The zoning is handled by your existing switches; your storage is off the existing switches, etc.

Why would you do this? By doing this, you could put CNAs in new servers (leaving the existing servers alone) to talk to the Nexus. This will simplify the server side infrastructure because you will have fewer cables, cards, switch ports, etc. Does the cost of the CNA and new infrastructure offset the cost of just continuing the old environment? That is for you to decide.

**_2. FCoE with a Nexus 5000 in a non-Cisco MDS environment (non-merging)_**

Who says you have to put the Nexus into the existing FC fabric? We have many customers that purchase "data centers in a box". By that I mean a few servers, FC and/or Ethernet switches, storage, and VMware all in one solution. This "box" sits in the data center and the network is merged with the legacy network, but we stand up a Cisco SAN next to the existing non-Cisco SAN and just not let them talk to each other. In this instance, we would use CNAs in the servers, Nexus as the switch, and you pick a storage vendor. This will work just like option 3.

**_3. FCoE with a Nexus 5000 in a Cisco MDS environment_**

Now we're talking. Install the Nexus in FC switch mode, merge it with the MDS fabric, put CNAs in all the servers and install the storage off the Nexus as either FC or FCoE. You're off to the races!

You potentially gain the same server side savings by replacing FC and Ethernet in new servers with CNAs. You are able to use all of the Cisco sexy features of FCoE. Nice solution if the cost is justified in your environment.

**_4. Keep the existing environment and use NFS to new servers_**

What did I just say? Why would I even consider that option?

OK, this last one is a little tongue-in-cheek for customers that are already using FC. The NFS vs. traditional storage for VMware is a bit of a religious debate. I know you aren't going to sway me and I know I'm not going to sway you.

I admit I'm thinking NetApp here in a VMware environment; I'm a big fan so this is a biased opinion. NetApp is my background but other vendors play in this space as well. I bet Chad will be happy to leave a comment to help tell us why (and I hope he does!).

Think of it this way. You're already moving from FC cards to CNAs. Why not buy regular 10Gb Ethernet cards instead? Why not just use the Nexus 5K as a line-rate, non-blocking 10Gb Ethernet switch? This configuration is very simple compared to FCoE at the Nexus level and management of the NetApp is very easy! Besides, you could always turn up FCoE on the Nexus (and the NetApp) at a future date.

In closing, I really like FCoE but as you can see it isn't a perfect fit _today_ for all environments. I really see this taking off in 1-2 years and I can't wait. Until then, use caution and ask all the right questions!

If you are interested in some more in-depth discussion, here are links to some of Scott's articles on FCoE:

[Continuing the FCoE Discussion][1]  

[Why No Multi-Hop FCoE?][2]  

[There Might Be an FCoE End to End Solution][3]

[1]: {{< relref "2008-12-09-continuing-the-fcoe-discussion.md" >}}
[2]: {{< relref "2009-08-11-why-no-multi-hop-fcoe.md" >}}
[3]: {{< relref "2009-07-30-correction-there-might-be-an-end-to-end-fcoe-solution.md" >}}
