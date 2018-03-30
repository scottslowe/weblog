---
author: slowe
categories: Explanation
comments: true
date: 2014-06-20T09:00:00Z
slug: learning-nsx-part-14-using-logical-routing
tags:
- Networking
- Neutron
- NSX
- NVP
- OpenStack
- OVS
- Virtualization
- VMware
title: 'Learning NSX, Part 14: Using Logical Routing'
url: /2014/06/20/learning-nsx-part-14-using-logical-routing/
wordpress_id: 3466
---

Welcome to part 14 of the Learning NSX blog series, in which I discuss the ability for VMware NSX to do Layer 3 routing in logical networks. This post will also include a look at a very cool feature within VMware NSX known as _distributed logical routing_. This post will take a closer look at distributed logical routing within the context of an OpenStack environment that's been integrated with VMware NSX. (Although NSX isn't necessarily tied to OpenStack, I'll assume you're using OpenStack just to simplify the discussion.)

If you're new to this series, you can find links to all the articles on [my Learning NVP/NSX page][all]. Ideally, I'd recommend you read _all_ the articles, but if you're just interested in some of the high-level concepts you probably don't need to do that. For those interested in the deep technical details, I'd suggest catching up on the series before proceeding.

## Overview of Logical Routing

One of the features of VMware NSX that can be useful, depending on customer requirements, is the ability to create complex network topologies. For example, creating a multi-tier network topology like the one shown below is easily accomplished via VMware NSX:

![Sample network topology](/public/img/logical-network-topology.png)

Note that this topology has two tenant-specific routing entities---these are _logical routers_. A logical router is an abstraction created and maintained by VMware NSX on behalf of your cloud management platform (like OpenStack, which I'll assume you're using here). These logical entities perform the routing process just like a physical router would (forwarding traffic based on a routing table, changing the source and destination MAC address, maintaining an ARP cache of MAC addresses, decrementing the TTL, etc.). Of course, they are not _exactly_ the same as physical routers; you can't, for example, connect two logical routers directly to each other.

Logical routers also act as the logical boundary between one or more logical networks and an external network. Logical routers can be connected to multiple logical networks (each logical network with its own logical router interface), but can only be connected to a single external network. Thus, you can't use a logical router as a transit path between two external networks (two VLANs, for example).

Now that you have a good understanding of logical routing, let's take a closer look at the various components inside VMware NSX.

## Components of Logical Routing

The components are pretty straightforward. In addition to the logical router abstraction that I've discussed already, you also have logical router ports (naturally, these are the ports on a logical router that connect it to a logical network or an external network), network address translation (NAT) rules (for handling address translation tasks), and a routing table (forwell, routing).

You can see all of these components in NSX Manager. Once you're logged into NSX Manager, select Network Components > Logical Layer > Logical Routers, then click on a specific logical router from the list. This will display the screen shown below (click the image for a larger version):

[![Logical router detail in NSX Manager](/public/img/part-14-lr-detail-small.png)](/public/img/part-14-lr-detail.png)

A few things to note here:

* You'll note that the logical router has a port whose attachment is listed as "L3GW". This denotes an attachment to a Layer 3 Gateway Service, an entity I described in [part 9][p9] of the series. This Layer 3 Gateway Service is itself comprised of two NSX gateway appliances; [part 6][p6] in the series discussed how to add a gateway appliance to your installation. The relationship between logical router, Layer 3 Gateway Service, and gateway appliance can be confusing for some; I plan to discuss that in more detail in the next post.

* This particular logical router is not configured as a distributed logical router. This means that the actual routing function resides on a Layer 3 Gateway Service. The routing functionality is instantiated in a highly available configuration on two different gateway appliances within the Layer 3 Gateway Service.

* NAT Synchronization is set to on; this refers to keeping NAT state synchronized between the active and standby routing functions instantiated on the gateway appliances.

* As noted under Replication Mode, this router uses an NSX service node (refer to [part 10][p10] for more details on service nodes) for packet replication/BUM traffic.

* You might notice that one of the logical router ports is assigned the IP address 169.254.169.253 (and you'll also note a corresponding "no NAT" rule and routing table entries for that same network). Astute readers recognize this as the network for Automatic Private IP Addressing (APIPA), also known as IPv4 Link-Local Addresses per RFC 3927. This exists to support an OpenStack-specific feature known as [the metadata service](http://docs.openstack.org/admin-guide-cloud/content/section_metadata-service.html), and is created automatically by OpenStack. (I'll talk more about OpenStack later in this post.)

All of these components and settings are accessible via the NSX API, and since NSX Manager is completely an API client (it merely consumes NSX APIs and does not provide standalone functionality outside of some logging features), you could create, modify, and delete any of the logical routing components directly within NSX Manager. (Or, if you were so inclined, you could make the API calls yourself to do these tasks.) Typically, though, these tasks would be handled via integration between NSX and your cloud management platform, like OpenStack.

One key component of NSX's logical routing functionality that you _can't_ see in NSX Manager is how the routing is actually implemented in the data plane. As with most features in NSX, the actual data plane implementation is handled via Open vSwitch (OVS) and a set of flow rules pushed down by the NSX controllers. These flow rules control the flow of traffic within and between logical networks (logical switches in NSX). You can see some of the flow rules in OVS using the `ovs-dpctl dump-flows` command, which will produce output something like what's shown in this screenshot (note that the addresses are highlighted because I used `grep` to show only the flows matching a certain IP address):

[![List of flows in OVS](/public/img/part-14-ovs-flow-list-small.png)](/public/img/part-14-ovs-flow-list.png)

_(Click the image above for a larger version.)_

These flow rules include actions like re-writing source and destination MAC addresses and decrementing the TTL, both tasks carried out by "normal" routers when routing traffic between networks. These flow rules also provide some insight into the differences between a logical router and a distributed logical router. While both are logical entities, the way in which the data plane is implemented is different for each:

* For a logical router, the flow rules will direct traffic to the appropriate gateway appliance in the Layer 3 Gateway Service. The logical router is actually instantiated on a gateway appliance, so all routed traffic must go to the logical router, get "routed" (routing table consulted, source and destination MAC re-written, TTL decremented, NAT rules applied, etc.), then get sent on to the final destination (which might be a VM on a hypervisor in NSX or might be a physical network outside of NSX).

* For a distributed logical router, the flow rules will direct traffic _either_ to the appropriate gateway appliance in the Layer 3 Gateway Service _or_ to the destination hypervisor directly. Why the "either/or"? If the traffic is north/south traffic---that is, traffic being routed out of a logical network onto the physical network---then it must go to the gateway appliance (which, as I have mentioned before, is where traffic is unencapsulated and placed onto the physical network). However, if the traffic is east/west traffic---traffic that is moving from one server on a logical network to another server on a logical network---then the traffic is "routed" directly on the source hypervisor and then sent across an encapsulated connection to the hypervisor where the destination VM resides.

In both cases, there is only **one** logical router. For a non-distributed logical router, the data plane is instantiated on a gateway appliance only. For a distributed logical router, the data plane is instantiated both on the local hypervisors as well as on a gateway appliance. (This is assuming you've set an uplink on the logical router, meaning you have a north/south connection. If you haven't set an uplink, then the routing functionality is instantiated on the hypervisors only.)

This should provide a good overview of how logical routing is implemented in VMware NSX, but there's one more aspect I want to cover: logical routers in OpenStack with NSX.

## Logical Routers in OpenStack

As you work with OpenStack Networking---Neutron, as it's commonly called---you'll find that the abstractions Neutron uses map really well to the abstractions that NSX uses. So, to create a logical router in NSX, you just create a logical router in OpenStack. Attaching an OpenStack logical router to a logical network tells NSX to create the logical switch port, create the logical router port, and connect the two ports together.

In OpenStack, there are a number of different ways to create a logical router:

* OpenStack Dashboard (Horizon)

* Command-line interface (CLI)

* OpenStack Orchestration (Heat) template

* API calls directly

When using the web-based Dashboard user interface, you can only create centralized logical routers, not distributed logical routers. The Dashboard UI also doesn't provide any way of knowing if a logical router is distributed or not; for that, you'll need the CLI (the command is provided shortly).

On a system with the `neutron` CLI client installed, you can create a logical router like this:

    neutron router-create <router name>

This creates a centralized logical router. If you want to create a distributed logical router, it's as simple as this:

    neutron router-create <router name> --distributed True

The `neutron router-show` command will return output about the specified logical router; that output will tell you if it is a distributed logical router.

The `neutron` CLI client also offers commands to update a logical router's routing table (to add or remove static routes, for example), or to connect a logical router to an external network (to set an uplink, in other words).

If you want to create a logical router as part of a stack created via OpenStack Orchestration (Heat), you could use this YAML snippet in a HOT-formatted template to create a distributed logical router (click [here](https://gist.github.com/scottslowe/6affeae4551e4fb0aafb) for an option to download this code snippet):

``` yaml
heat_template_version: 2013-05-23
description: >
  A simple Heat template to create a distributed logical router.
resources:
  router0:
    type: OS::Neutron::Router
    properties:
      admin_state_up: True
      name: distributed-router
      value_specs: { distributed: "True" }
```

OpenStack Heat also offers resource types for setting the router's external gateway and creating router interfaces (logical router ports). If you aren't familiar with OpenStack Heat, you might find [this introduction][1] useful.

That wraps up this post on logical routing with VMware NSX. As always, I welcome your courteous feedback, so feel free to speak up in the comments below. In the next post, I'll spend a bit of time discussing logical routers, gateway servies, and gateway appliances. See you next time!

[all]: /learning-nvp-nsx/
[p9]: {{< relref "2014-02-26-learning-nsx-part-9-adding-a-gateway-service.md" >}}
[p6]: {{< relref "2013-10-28-learning-nvp-part-6-adding-an-nvp-gateway.md" >}}
[p10]: {{< relref "2014-02-27-learning-nsx-part-10-adding-a-service-node.md" >}}
[1]: {{< relref "2014-05-01-an-introduction-to-openstack-heat.md" >}}
