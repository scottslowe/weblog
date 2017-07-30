---
author: slowe
categories: Information
comments: true
date: 2009-09-02T17:50:29Z
slug: some-additional-vmworld-vendor-meetings
tags:
- FibreChannel
- Storage
- Virtualization
- VMware
- VMworld2009
title: Some Additional VMworld Vendor Meetings
url: /2009/09/02/some-additional-vmworld-vendor-meetings/
wordpress_id: 1639
---

Earlier I posted some notes on meetings I'd had with Virsto and Xangati. In this post I'd like to discuss some additional meetings I've had with Virtual Instruments and Tranxition.

## Virtual Instruments

[Virtual Instruments](http://www.virtualinstruments.com/) makes a solution that is intended to help troubleshoot and optimize storage environments. I had the opportunity to grab some coffee with them this morning and hear about what they're doing and how they're doing it. As a company carved out of Finisar and taken private, their goal is to help drive higher levels of virtualization by providing more visibility into the storage fabric.

Clearly, this message will really only resonate with larger customers, and that is their target market: multiple hundreds of terabytes into the single petabyte range. At this scale, providing visibility into the thousands of virtual machines across hundreds of ESX/ESXi hosts attached to hundreds of Fibre Channel ports is almost impossible. Virtual Instruments tackles this with a multi-prong approach:

* First, they use a SAN tap to plug into the Fibre Channel fabric and mirror traffic information to a collection device for analysis. If you're a networking person, you can think of this as using a SPAN port to mirror traffic. This is done on the storage side to reduce the scale due to fan in-fan out ratios.

* Second, they gather SNMP information from the Fibre Channel switches. This enables visibility at the switch level.

* Third and finally, Virtual Instruments collects information from VMware vCenter Server. This information provides the final piece necessary to correlate per-host and per-VM traffic to the information being gathered by the fabric taps and the switch monitoring.

What this allows Virtual Instruments to do is to feed information back to vCenter Server to enable I/O-based recommendations for VM movement. It also enables visibility into path utilization so that multipathing information can be configured for optimal performance. Finally, more detailed storage information is exposed that enables organizations to more effectively place VM storage on Tier 1, Tier 2, or Tier 3 according to its storage needs. In some cases, in fact, money saved on buying additional Tier 1 storage can more than pay for an implementation of Virtual Instruments.

Overall, this is very interesting soltuion, albeit limited in scope to larger environments. If this describes your organization, though, it may definitely be worth a closer look.

## Tranxition

[Tranxition](http://www.tranxition.com/index.shtml) makes software to do "personality virtualization." Apparently they've been around since 1998 and are just now becoming more visible, creating a partner program, and starting to expand coverage. Their key product is Adaptive Persona, which some have said can be called "Softricity for user personality data". The product seems to work a lot like ThinApp in that it creates a virtual file system and virtual Registry that captures all user personality data. This user personality data, which can reside either inside or outside the traditional user profile file system structure, is then continuously streamed back to a central server. When a user logs off, whatever data has not been synchronized to the server is then copied up to the server, and the local system is scrubbed of user personality data. Then, when that same user logs on to a different system, Tranxition streams down only those portions of the user personality that are needed at that moment. All other data is fetched "on demand". This helps speed up the logon process by decoupling the size of the profile from the time required to log on.

Overall, I was fairly impressed with the product. They seem to have done a reasonably good job of taking the principles behind application virtualization and applied them to user personality management. If anyone has any additional feedback on Tranxition (vendors, please disclose yourselves!), I'd love to hear it in the comments.
