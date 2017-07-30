---
author: slowe
categories: Explanation
comments: true
date: 2008-08-06T23:16:07Z
slug: a-few-srm-discussion-points
tags:
- Storage
- Virtualization
- VMware
- VMwareSRM
title: A Few SRM Discussion Points
url: /2008/08/06/a-few-srm-discussion-points/
wordpress_id: 802
---

I've never really discussed VMware Site Recovery Manager (SRM) here; there always seemed to be plenty of coverage elsewhere. Just recently, though, I had the opportunity to spend some time with a very knowledgeable SRM resource within VMware, and gathered some notes about VMware SRM that I thought might be helpful. Some of this stuff may be obvious, so bear with me.

* Storage array replication is a **necessity**. Without it, SRM can't be used. Keep in mind that only certain arrays and certain replication technologies are supported, so be sure to check the SRM compatibility list.

* The Storage Recovery Adapter (SRA) is a critical part of an SRM deployment, but it doesn't come from VMware. It comes from the storage vendor (assuming that it is a compatible array and compatible replication technology).

* Two instances of VirtualCenter (VC) are required. One of these will be at the "Protected Site," the other will be at the "Recovery Site."

* Likewise, two instances of SRM are needed, one at each site.

* The VC servers and SRM servers at each site need to be able to talk with each other, i.e., they need IP-based connectivity. SRM will communicate with the local VC server over TCP ports 443 and 8095. SRM will communicate with the remote VC server over TCP port 443. The local SRM server uses the remote VC server as a proxy to communicate with the remote SRM server instead of communicating with it directly.

* VC and SRM each require their own database.

* If the physical hardware is sufficiently equipped, then VC and SRM can be co-located on the same server. Otherwise, VC and SRM should be placed on their own physical server.

* SRM does not support failback. Instead, create a Recovery Plan in reverse.

* The VC and SRM databases do not replicate between the sites. They are maintained separately.

* Observe the "DNS Rule of Four" for SRM---forward lookup, reverse lookup, short name, and fully-qualified domain name (FQDN). All four of these should work properly.

* All VMs in a Protection Group will fail over at the same time, so users will want to properly architect the Protection Groups to provide the appropriate DR functionality for the right VMs. Application dependencies are important here---failing over some VMs but not others that provide dependency services won't do much good, now will it?

* VMware SRM requires Virtual Center 2.5, and VirtualCenter 2.5 Update 1 is recommended. Update 2 is not supported.

* Similarly, VMware ESX 3.5 Update 2 is also not supported (yet).

I'm confident I'll have more posts on VMware SRM in the coming months. In the meantime, feel free to add your thoughts in the comments below.
