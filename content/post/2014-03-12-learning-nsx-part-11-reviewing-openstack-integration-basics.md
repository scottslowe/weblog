---
author: slowe
categories: Explanation
comments: true
date: 2014-03-12T09:00:00Z
slug: learning-nsx-part-11-reviewing-openstack-integration-basics
tags:
- Networking
- Neutron
- NSX
- NVP
- OpenStack
- Virtualization
- VMware
title: 'Learning NSX, Part 11: Reviewing OpenStack Integration Basics'
url: /2014/03/12/learning-nsx-part-11-reviewing-openstack-integration-basics/
wordpress_id: 3421
---

Welcome to part 11 of the Learning NSX blog series, in which I provide a high-level overview of the basics on integrating VMware NSX into an [OpenStack](http://openstack.org/) deployment using OpenStack Neutron. In case you're just now catching up on this blog series, I encourage you to visit [my Learning NVP/NSX page][1], which has a brief summary of all the posts in the series.

Now that I've shown you how to build all the different components of NSX ([NSX controllers][2], [NSX Manager][3], [gateway appliances][4], and [service nodes][5]), it's time to add integration into a cloud management platform. VMware NSX was designed to be integrated into a cloud management platform. Because VMware NSX offers a full-featured RESTful API, you could---in theory---integrate NSX into just about _any_ cloud management platform. However, for the purposes of this series, I'll limit the discussion to focus on how one would integrate VMware NSX with OpenStack via the OpenStack Neutron (formerly "Quantum") project, which provides virtual networking functionality for OpenStack-based clouds.

The challenge in discussing OpenStack-NSX integration is that one must first understand the basics of OpenStack Neutron before looking at how to integrate VMware NSX into Neutron. Therefore, the primary goal of this post in the series is to provide an overview of Neutron, and then discuss how NSX integrates into Neutron. The next post in the series will provide more in-depth technical details on exactly how the integration is configured.

Let's start with a generic overview of OpenStack Neutron and its components.

## Examining OpenStack Neutron Components

OpenStack Neutron itself has a number of different components:

* The Neutron server, which supplies the Neutron API and is typically---but not required to be---deployed co-resident on an OpenStack "controller node" (not to be confused with an NSX controller).

* The Neutron DHCP agent, which provides DHCP services for the various logical networks created in Neutron (at least, whenever DHCP is enabled for a logical network).

* The Neutron L3 agent, which provides L3 (routed) connectivity for Neutron logical networks. This includes both L3 between logical networks as well as L3 in and out of logical networks.

* The Neutron metadata agent, which is responsible for providing connectivity between instances and the Nova metadata service (for customizing instances appropriately).

* Finally, for many Open vSwitch (OVS)-based installations, there is the Neutron OVS agent, which provides a way to program OVS to do what Neutron needs it to do.

The Neutron DHCP, L3, and metadata agents are typically---but not required to be---installed on a so-called "network node." This network node provides DHCP services to the various logical networks (often using [Linux network namespaces][6], if the Linux distribution supports them), routed connectivity in and out of logical networks (once again with network namespaces, Linux bridges, and iptables rules), and metadata service connectivity.

From a traffic flow perspective, traffic from one subnet in a logical network to another subnet in a logical network must hairpin through the network node (where the L3 agent resides). Traffic headed out of a logical network must flow through the network node, where iptables rules will perform the necessary network address translation (NAT) functions associated with the use of floating IPs in an OpenStack environment. And, as I've already mentioned, the network node provides DHCP services to all the logical networks as well.

This is, by necessity, a very high-level overview of OpenStack Neutron and the core components. Let's now take a look at how these components are affected when you choose to use VMware NSX with OpenStack Neutron.

## Reviewing OpenStack Neutron With NSX

When you're using VMware NSX as the mechanism behind OpenStack Neutron, some of Neutron's functionality is provided by NSX itself:

* You no longer need the L3 agent. L3 connectivity is provided by the NSX gateway appliances and logical gateway services (refer to [part 6][4] and [part 9][7] of the series, respectively).

* You won't need the OVS agent on the hypervisors, because the NSX controllers are responsible for configuring/programming OVS on transport nodes. More details on this interaction is provided in [part 4][8] of the series. (_Transport nodes_ is a generic term referring to nodes that participate in the data plane, such as hypervisors, gateways, and service nodes.)

* You will still need the Neutron server, the DHCP agent, and the metadata agent.

Therefore, if you choose to deploy the OpenStack Neutron components in a "typical" fashion, you'd have a setup something like this:

* An OpenStack "controller node" would host the Neutron server, which provides the API with which other parts of OpenStack will interact. This node generally does not have OVS installed, but would have the NSX plugin for Neutron installed. This plugin implements the integration between OpenStack and NSX, and will communicate with NSX via the NSX northbound RESTful API.

* An OpenStack "network node" would host the Neutron DHCP agent and the Neutron metadata agent. This node would have OVS installed, and would be registered into NSX as a hypervisor (even though it is not a hypervisor and will not host any VMs).

At this point, you should have a pretty good understanding of how, at a high level, NSX integrates with and affects OpenStack Neutron. In the next post in the series, I'll provide more details on exactly how to configure the integration between VMware NSX and OpenStack Neutron.

[1]: /learning-nvp-nsx/
[2]: {{< relref "2013-08-16-learning-nvp-part-2-nvp-controllers.md" >}}
[3]: {{< relref "2013-08-19-learning-nvp-part-3-nvp-manager.md" >}}
[4]: {{< relref "2013-10-28-learning-nvp-part-6-adding-an-nvp-gateway.md" >}}
[5]: {{< relref "2014-02-27-learning-nsx-part-10-adding-a-service-node.md" >}}
[6]: {{< relref "2013-09-04-introducing-linux-network-namespaces.md" >}}
[7]: {{< relref "2014-02-26-learning-nsx-part-9-adding-a-gateway-service.md" >}}
[8]: {{< relref "2013-08-22-learning-nvp-part-4-adding-hypervisors-to-nvp.md" >}}
