---
author: slowe
categories: Explanation
comments: true
date: 2014-07-16T09:00:00Z
slug: learning-nsx-part-15-nsx-gateways-gateway-services-and-logical-routers
tags:
- Networking
- Neutron
- NSX
- NVP
- OpenStack
- Virtualization
- VMware
title: 'Learning NSX, Part 15: NSX Gateways, Gateway Services, and Logical Routers'
url: /2014/07/16/learning-nsx-part-15-nsx-gateways-gateway-services-and-logical-routers/
wordpress_id: 3470
---

This is part 15 of my Learning NSX blog series, in which I will spend some time diving a bit deeper into some of the components involved in the logical routing process I described in part 14. Specifically, I'll be taking a deeper look at gateway appliances, gateway services, and logical routers, and the relationships among these various components.

If you haven't read any of the prior posts in this series, it would be ideal to read all of them before continuing; you can find links on [my Learning NVP/NSX page][all]. In particular, I'd suggest reading [part 6][p6] (on adding a gateway appliance), [part 9][p9] (on adding a gateway service), and [part 14][p14] (on logical routing and logical routers).

Just for the sake of completeness and to reinforce what was introduced in those posts I referenced, let's start with some terminology:

* _Gateway (or gateway appliance):_ When I use the terms _gateway_ or _gateway appliance_, I'm referring to the NSX software gateway that acts as the "on-ramp/off-ramp" to and from logical networks. What makes this confusing is that we also use the term "gateway" (in particular, "IP gateway" or "default gateway") to refer to a Layer 3 router that acts as the next hop for a aystem. I'll do my best to make sure that I'm clearly distinguishing between these ambiguous uses.

* _Gateway service:_ A _gateway service_ is a logical construct within NSX that allows you to group together multiple _gateway appliances_. For example, in an L2 gateway service, you can combine two gateway appliances so that you have redundancy in providing L2 bridging functionality between a logical network and a physical network. In an L3 gateway service, you can combine up to 10 gateway appliances together for redundancy and scale-out performance.

* _Logical router:_ As you might recall from part 14, a logical router is a logical construct within NSX that provides Layer 3 routing functionality, typically (but not always) on a per-tenant basis.

I have a few more terms I'll introduce in this post, but that should be enough for now.

This diagram contains the bulk of what I'd like to discuss in this post---the relationship between gateway services, gateway appliances, and logical routers:

![Gateway service, gateway appliance, and logical router relationships](/public/img/part-15-gws-gwa-lr.png)

As I walk you through the details of this diagram, hopefully I'll clarify the relationships between these components.

* In this example, there are four gateway appliances combined into a single Layer 3 gateway service. As illustrated in the diagram, gateway services can contain more than one gateway appliance (the minimum recommended is two, for reasons to be explained shortly). Gateway services may be either Layer 2 (bridging/switching) or Layer 3 (routing), but not both.

* A gateway appliance may be a member of only one gateway service at a time; therefore, a gateway appliance is either L2 or L3, but not both.

* When adding a gateway appliance to a gateway service, the administrator or operator has the ability to specify a _failure zone ID_. The idea behind the failure zone ID is to help model fault domains within a single gateway service. For example, if GW Appliance 1 is in a different fault domain---say, a different rack---then the administrator or operator could assign a different failure zone ID to GW Appliance 1, indicating that GW Appliance 1 is in a different fault domain. The significance of this functionality will be made clear in a moment.

* Note that gateway services, gateway appliances, and failure zone IDs are **not** visible to tenants. Further, the configuration or management of these entities is handled through NSX (via API or NSX Manager), and isn't tenant-specific. The CMP---OpenStack, for example---doesn't get involved here.

* The example diagram shows four different logical routers spread across three tenants. Each of these logical routers acts as an IP gateway (default gateway/default route) for the associated (or connected) logical network(s). Thus, a logical router **is** visible to a tenant.

* Creating, managing, and configuring logical routers is handled by the CMP. With OpenStack, for example, you'd use the OpenStack Dashboard or the Neutron command-line client.

* For redundancy, you'll note that each logical router is instantiated on 2 different gateway appliances within the gateway service (hence why a minimum of 2 gateway appliances within a gateway service is recommended). This is completely invisible to the tenant and is handled automatically by NSX. If failure zone IDs---indicating different fault domains---are configured on the gateway appliances, then NSX will instantiate the logical router on gateway appliances _in different failure zones_. This is an attempt to minimize downtime by spreading the logical router across fault domains.

So far, everything I've shared with you has been true for centralized logical routers. For distributed logical routers, things are only _slightly_ different. Distributed logical routers are normally instantiated on the hypervisors; a gateway service and its associated gateway appliances only gets involved when you set the uplink for the distributed logical router (using the "Set Gateway" button in OpenStack Dashboard, for example). If you never set an uplink for the logical router, it will remain instantiated only on the hypervisors, and not on the gateway service/gateway appliances.

I hope this information helps in understanding the routing aspects of VMware NSX. Feel free to post any questions, clarifications, or thoughts in the comments below. Any input on other topics you'd like to see in the Learning NSX blog series are welcome as well!

[all]: /learning-nvp-nsx/
[p6]: {{< relref "2013-10-28-learning-nvp-part-6-adding-an-nvp-gateway.md" >}}
[p9]: {{< relref "2014-02-26-learning-nsx-part-9-adding-a-gateway-service.md" >}}
[p14]: {{< relref "2014-06-20-learning-nsx-part-14-using-logical-routing.md" >}}
