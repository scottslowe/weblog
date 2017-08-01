---
author: slowe
categories: Tutorial
comments: true
date: 2014-02-26T13:54:15Z
slug: learning-nsx-part-9-adding-a-gateway-service
tags:
- Networking
- NSX
- NVP
- Virtualization
- VMware
title: 'Learning NSX, Part 9: Adding a Gateway Service'
url: /2014/02/26/learning-nsx-part-9-adding-a-gateway-service/
wordpress_id: 3407
---

Welcome to part 9 in the Learning NVP/NSX blog series, in which I'll discuss adding a gateway service to a logical network.

If you are just now joining me for this series, let me bring you up to speed real quick:

* Part 1 provided a [high-level overview of NVP/NSX][1] and its core components. (Reminder: NVP's core components are much the same as NSX's core components in multi-hypervisor environments.)

* Part 2 showed you how to [build NVP/NSX controllers and configure them into a controller cluster][2].

* Part 3 walked you through the process to [install and configure NVP/NSX Manager][3], a web-based GUI that you can use to configure certain aspects of NVP/NSX.

* Part 4 took you through the process of [adding hypervisors to NVP/NSX][4].

* Part 5 showed you how to [create a logical network][5] that could be used to connect VMs to each other independent of the underlying physical network topology.

* Part 6 stepped through [adding an NVP/NSX gateway appliance][6] to your environment, setting the stage for you to later add L2/L3 connectivity in and out of your logical networks.

* [Part 7][7] and [part 8][8] focused on the transition of this blog series from NVP to NSX; no substantially new information was shared.

This installation in the series builds upon all the previous articles; in particular, I assume that you have a logical network created and configured and that you have an NSX gateway appliance up running and added to the NSX domain. In this post, I'll show you how to add a logical gateway service to your network so that you can provide routed (layer 3) connectivity into and out of your logical network.

Before I start, I think it's important to distinguish between a gateway appliance and a gateway service. A _gateway appliance_ is a physical installation (or a VM; it's supported as a VM in certain configurations) of the NSX gateway software; this is what I showed you how to set up in [part 6][6] of the series. A _gateway service_, on the other hand, is a logical construct. Typically, you would have multiple gateway appliances. Then you would create a gateway service that would be instantiated across multiple gateway appliances for redundancy and availability. Gateway services can be either layer 2 (provided bridged connectivity between a logical network and VLANs on the physical network) or layer 3 (providing routed---with or without NAT---connectivity between a logical network and the physical network).

It's also important to understand the distinction between a gateway service and a logical router. A gateway service is an NSX construct; a _logical router_, on the other hand, is usually a construct of the cloud management platform (like OpenStack). A single gateway service can host many logical routers.

In this series, I'm only going to focus on layer 3 gateway services (primarily because I have limited resources in my environment and can't run both layer 2 and layer 3 gateway services).

To create a layer 3 gateway service, you'll follow these steps from within NSX Manager (formerly NVP Manager, which I showed you how to set up in [part 3][3]):

1. From the menu across the top of the NSX Manager page, select Network Components > Services > Gateway Services. This will take you to a page titled "Network Components Query Results," where NSX Manager has precreated and executed a query for the list of gateway services. Your list will be empty, naturally.

2. Click the Add button. This will open the Create Gateway Service dialog.

3. Select "L3 Gateway Service" from the list. Other options in this list include "L2 Gateway Service" (to create a layer 2 gateway service) and "VTEP L2 Gateway Service" (to integrate a third-party top-of-rack [ToR] switch into NSX). Click Next, or click on the "2. Basics" button on the left.

4. Provide a display name for the new layer 3 gateway service, then click Next (or click on "3. Transport Nodes" on the left). You can optionally add tags here as well, in case you wanted to associate additional metadata with this logical object in NSX.

5. On the Transport Nodes screen, click Add Gateway to select a gateway appliance (which is classified as a transport node within NSX; hypervisors are also transport nodes) to host this layer 3 gateway service.

6. From the Edit Gateway dialog box that pops up, you'll need to select a transport node, a device ID, and a failure zone ID. The first option, the transport node, is pretty straightforward; this is a gateway appliance on which to host this gateway service. The device ID is the bridge (recall that NSX gateway appliances, by default, create OVS bridges to map to their interfaces) connected to the external network. The failure zone ID lets you "group" gateway appliances with regard to their relationship to the gateway service. For example, you could choose to host a gateway service on a gateway from failure zone 1 as well as failure zone 2. (Failure zones are intended to help represent different failure domains within your data center.)

7. Once you've added at least two gateway appliances as transport nodes for your gateway service, click Save to create the gateway service and return to NSX Manager. That's it---you're done!

Note that this is more of an implementation task than an operational task. In other words, you'd generally deploy your gateway services when you first set up NSX and your cloud management platform, or when you are adding capacity to your environment. This isn't something that you have to do when a cloud tenant (customer) needs a logical router; that's handled automatically through the integration between NSX and the cloud management platform. As I mentioned earlier, a single layer 3 gateway service could host many logical routers.

In the next installation of the series, I'll walk you through setting up an NSX service node to offload packet replication (for broadcast, unknown unicast, and multicast traffic).

As always, your feedback is welcome and encouraged, so feel free to speak up in the comments below.

[1]: {{< relref "2013-05-21-learning-nvp-part-1-high-level-architecture.md" >}}
[2]: {{< relref "2013-08-16-learning-nvp-part-2-nvp-controllers.md" >}}
[3]: {{< relref "2013-08-19-learning-nvp-part-3-nvp-manager.md" >}}
[4]: {{< relref "2013-08-22-learning-nvp-part-4-adding-hypervisors-to-nvp.md" >}}
[5]: {{< relref "2013-09-06-learning-nvp-part-5-creating-a-logical-network.md" >}}
[6]: {{< relref "2013-10-28-learning-nvp-part-6-adding-an-nvp-gateway.md" >}}
[7]: {{< relref "2013-11-01-learning-nvp-part-7-handling-the-nvp-to-nsx-transition.md" >}}
[8]: {{< relref "2013-12-05-learning-nvp-part-8-an-update-on-the-nvp-to-nsx-transition.md" >}}
