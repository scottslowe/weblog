---
author: slowe
categories: Tutorial
comments: true
date: 2014-10-13T09:00:00Z
slug: learning-nsx-part-16-routing-to-multiple-external-vlans
tags:
- Networking
- Neutron
- NSX
- OpenStack
- Virtualization
- VLAN
- VMware
title: 'Learning NSX, Part 16: Routing to Multiple External VLANs'
url: /2014/10/13/learning-nsx-part-16-routing-to-multiple-external-vlans/
wordpress_id: 3544
---

This is part 16 of the Learning NSX series, in which I will show you how to configure VMware NSX to route to multiple external VLANs. This configuration will allow you to have logical routers that could be uplinked to any of the external VLANs, providing additional flexibility for consumers of NSX logical networks.

Naturally, this post builds on all the previous entries in this series, so I encourage you to visit [the Learning NVP/NSX page][all] for links to previous posts. Because I'll specifically be discussing NSX gateways and routing, there are some posts that are more applicable than others; specifically, I strongly recommend reviewing [part 6][p6], [part 9][p9], [part 14][p14], and [part 15][p15]. Additionally, I'll assume you're using VMware NSX with OpenStack, so reviewing [part 11][p11] and [part 12][p12] might also be helpful.

Ready? Let's start with a very quick review.

## Review of NSX Gateway Connectivity

You may recall from [part 6][p6] that the NSX gateway appliance is the piece of VMware NSX that handles traffic into or out of logical networks. As such, the NSX gateway appliance is something of a "three-legged" appliance:

* One "leg" (network interface) provides management connectivity among the gateway appliance and the nodes in the NSX controller cluster

* One "leg" provides connectivity to the transport network, which carries the encapsulated logical network traffic

* One "leg" is the uplink and provides connectivity to physical networks

That's the physical architecture. From a more logical architecture, you may recall from [part 15][p15] that NSX gateway appliances are combined into an NSX gateway service, and the NSX gateway service hosts one or more logical routers. Neither the NSX gateway appliance nor the NSX gateway service are visible to the consumers of the environment; they are only visible to the operators and/or administrators. Consumers only see logical routers, which also serve as the default gateway/default route/IP gateway to/from their logical networks.

The configurations I've shown you/discussed so far have assumed the presence of only a single uplink. NSX is not constrained to having only a single uplink, nor is it constrained to having only a single physical network on an uplink. If you need multiple networks on the outside of an NSX gateway appliance, you can either use multiple uplinks, or you can use multiple VLANs on an uplink. In this post I'll show you how to use multiple VLANs on the outside. This diagram provides a graphical representation of what the configuration will look like.

![Multiple VLANs with NSX Gateways](/public/img/part-16-nsx-gw-multi-vlan-small.png)

(Click [here](/public/img/part-16-nsx-gw-multi-vlan.png) for a larger version.)

Setting up this configuration will involve three steps:

1. Configuring the uplink to carry multiple VLANs.

2. Verifying the gateway configuration.

3. Setting up the external networks in OpenStack.

Let's take a look at each of these sections.

## Configuring Multiple VLANs on the Gateway Uplink

The process for this step will vary, mostly because it involves configuring your physical network to pass the appropriate VLANs to the NSX gateway appliance. I've written a few articles in the past that might be helpful here:

* [ESX Server, NIC Teaming, and VLAN Trunking][1] (for Cisco environments)

* [Link Aggregation and VLAN Trunking with Brocade FastIron Switches][2] (for Brocade/Foundry environments)

* [VMware ESX, NIC Teaming, and VLAN Trunking with HP ProCurve][3] (for HP environments)

Although the titles of some of these articles seem to imply they are VMware-specific, they aren't---the physical switch configuration is absolutely applicable here.

## Verifying the Gateway Configuration

No special configuration is required on the NSX gateway appliance. As you probably already know, the NSX gateway appliance leverages Open vSwitch (OVS). OVS ports are, by default, trunk ports, and therefore will carry the VLAN tags passed by a properly configured physical switch. Further, the OVS bridge for the external uplink (typically `breth1` or `breth2`) doesn't need an IP address assigned to it. This is because the IP address(es) for logical routing are assigned to the logical routers, not the NSX gateway appliance's interface. If you do have IP addresses assigned to the external uplink interface, you can safely remove it. If you prefer to leave it, that's fine too.

As a side note, the NSX gateway appliances do support configuring VLAN sub-interfaces using a command like this:

    add network interface <physical interface> vlan <VLAN ID>

Thus far, I haven't found a need to use VLAN sub-interfaces when using multiple VLANs on the outside of an NSX gateway appliance, but I did want to point out that this functionality does indeed exist.

## Setting up the External Networks

This is the only moderately tricky part of the configuration. In this step, you'll prepare multiple external networks that can be used as uplinks for logical routers.

The command you'll want to use (yes, you _have_ to use the CLI---this functionality isn't exposed in the OpenStack Dashboard web interface) looks like this:

    neutron net-create <network name> -- 
    --router:external=True --provider:network_type l3_ext 
    --provider:segmentation_id <VLAN ID> 
    --provider:physical_network=<NSX gateway service UUID> --shared=True

For the most part, this command is pretty straightforward, but let's break it down nevertheless:

* The `router:external=True` tells Neutron this network can be used as the external (uplink) connection on a logical router.

* The `provider:network_type l3_ext` is an NSX-specific extension that enables Neutron to work with the layer 3 (routing) functionality of the NSX gateway appliances.

* The `provider:segmentation_id` portion provides the VLAN ID that should be associated with this particular external network. This VLAN ID should be one of the VLAN IDs that is trunked across the connection from the physical switch to the NSX gateway appliance.

* The `provider:physical_network` portion tells OpenStack which specific NSX gateway service to use. This is important to note: this command references an NSX gateway service, _not an NSX gateway appliance_. Refer to [part 15][p15] if you're unclear on the difference.

You'd repeat this command for each external network (VLAN) you want connected to NSX and usable inside OpenStack.

For each Neutron network, you'll also need a Neutron subnet. The command to create a subnet on one of these external networks looks like this:

    neutron subnet-create <network name> <CIDR>
    --name <subnet name> --enable_dhcp=False 
    --allocation-pool start=<starting IP address>,end=<ending IP address>

The range of IP addresses specified in the `allocation_pool` portion of the command becomes the range of addresses from this particular subnet that can be assigned as floating IPs. It is also the pool of addresses from which logical routers will pull an address when they are connected to this particular external network.

When you're done creating an external network and subnet for each VLAN on the outside of the NSX gateway appliance, then your users (consumers) can simply create logical routers as usual, and then select from one of the external networks as an uplink for their logical routers. This assumes you included the `shared=True` portion of the command when creating the network; if desired, you can omit that and instead specify a tenant ID, which would assign the external network to a specific tenant only.

I hope you find this post to be useful. If you have any questions, corrections, or clarifications, please speak up in the comments. All courteous comments are welcome!

[all]: /learning-nvp-nsx/
[p6]: {{< relref "2013-10-28-learning-nvp-part-6-adding-an-nvp-gateway.md" >}}
[p9]: {{< relref "2014-02-26-learning-nsx-part-9-adding-a-gateway-service.md" >}}
[p14]: {{< relref "2014-06-20-learning-nsx-part-14-using-logical-routing.md" >}}
[p15]: {{< relref "2014-07-16-learning-nsx-part-15-nsx-gateways-gateway-services-and-logical-routers.md" >}}
[p11]: {{< relref "2014-03-12-learning-nsx-part-11-reviewing-openstack-integration-basics.md" >}}
[p12]: {{< relref "2014-04-25-learning-nsx-part-12-integrating-vmware-nsx-with-openstack.md" >}}
[1]: {{< relref "2006-12-04-esx-server-nic-teaming-and-vlan-trunking.md" >}}
[2]: {{< relref "2012-10-26-link-aggregation-and-vlan-trunking-with-brocade-fastiron-switches.md" >}}
[3]: {{< relref "2008-09-05-vmware-esx-nic-teaming-and-vlan-trunking-with-hp-procurve.md" >}}
