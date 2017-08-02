---
author: slowe
categories: Tutorial
comments: true
date: 2014-10-27T09:00:00Z
slug: learning-nsx-part-17-adding-external-l2-connectivity
tags:
- Networking
- Neutron
- NSX
- OpenStack
- Virtualization
- VMware
- Security
title: 'Learning NSX, Part 17: Adding External L2 Connectivity'
url: /2014/10/27/learning-nsx-part-17-adding-external-l2-connectivity/
wordpress_id: 3553
---

This is part 17 of the Learning NSX blog series. In this post, I'll show you how to add layer 2 (L2) connectivity to your NSX environment, and how to leverage that L2 connectivity in an NSX-powered OpenStack implementation. This will allow you, as an operator of an NSX-powered OpenStack cloud, to offer L2/bridged connectivity to your tenants as an additional option.

As you might expect, this post does build on content from previous posts in the series. Links to all the posts in the series are available on [the Learning NVP/NSX page][all]; in particular, this post will leverage content from [part 6][p6]. Additionally, I'll be discussing using NSX in the context of OpenStack, so reviewing [part 11][p11] and [part 12][p12] might also be helpful.

There are 4 basic steps to adding L2 connectivity to your NSX-powered OpenStack environment:

1. Add at least one NSX gateway appliance to your NSX implementation. (Ideally, you would add two NSX gateway appliances for redundancy.)

2. Create an NSX L2 gateway service.

3. Configure OpenStack for L2 connectivity by configuring Neutron to use the L2 gateway service you just created.

4. Add L2 connectivity to a Neutron logical network by attaching to the L2 gateway service.

Let's take a look at each of these steps. (By the way, if the concept of "L2 connectivity" doesn't make sense to you, please review [part 1 of my "Introduction to Networking"][1] series.)

## Adding an NSX Gateway Appliance

I described the process for adding an NSX gateway appliance in [part 6][p6] of the series, so refer back to that article for details on how to add an NSX gateway appliance. The process for adding a gateway appliance is the same regardless of whether you'll use that gateway appliance for L2 (bridged) or L3 (routed) connectivity.

A few things to note:

* Generally, your gateway appliance will have at least three (3) network interfaces. One of those interfaces will be used for management traffic, one for transport (overlay) traffic, and one for external traffic. You'll need to assign IP addresses to the management and transport interfaces, but the external interface does _not_ require an IP address.

* If you are going to use the gateway appliance to provide L2 connectivity to multiple VLANs, you'll want to ensure that all appropriate VLANs are trunked to the external interface of the gateway appliances. If you are deploying redundant gateway appliances, make sure all the VLANs are trunked to all appliances.

Once you have the gateway appliance built and added to NSX using the instructions in [part 6][p6], you're ready to proceed to the next step.

## Creating an NSX L2 Gateway Service

After your gateway appliances (I'll assume you're using two appliances for redundancy) are built and added to NSX, you're ready to create the L2 gateway service that will provide the L2 connectivity in and out of a NSX-backed logical network. This process is similar to the process described in [part 9][p9] of the series, which showed you how to add an L3 gateway service to NSX. (If you're unclear on the difference between a gateway appliance and a gateway service, check out [part 15][p15] for a more detailed explanation.)

Before we walk through creating an L2 gateway service, keep in mind that you may connect _either_ an L3 gateway service _or_ an L2 gateway service to a single broadcast domain on the physical network. Let's say you connect an L3 gateway service to VLAN 100 (perhaps using multiple VLANs as described in [part 16][p16]). You can't also connect an L2 gateway service to VLAN 100 as well; you'd need to use a different VLAN on the outside of the L2 gateway service. Be sure to take this fact into account in your designs.

To create an L2 gateway service, follow these steps from within NSX Manager:

1. From the menu across the top of the NSX Manager page, select Network Components > Services > Gateway Services. This will take you to a page titled "Network Components Query Results," where NSX Manager has precreated and executed a query for the list of gateway services. Your list may or may not be empty, depending on whether you've created other gateway services. Any gateway services that you've already created will be listed here.

2. Click the Add button. This will open the Create Gateway Service dialog.

3. Select "L2 Gateway Service" from the list. Other options in this list include "L3 Gateway Service" (you saw this in [part 9][p9]) and "VTEP L2 Gateway Service" (to integrate a third-party top-of-rack [ToR] switch into NSX; you'll use this in a future post). Click Next, or click on the "2. Basics" button on the left.

4. Provide a display name for the new L2 gateway service, then click Next (or click on "3. Transport Nodes" on the left). You can optionally add tags here as well, in case you wanted to associate additional metadata with this logical object in NSX.

5. On the Transport Nodes screen, click Add Gateway to select a gateway appliance (which is classified as a transport node within NSX; hypervisors are also transport nodes) to host this L2 gateway service.

6. From the Edit Gateway dialog box that pops up, you'll need to select a transport node and a device ID. The first option, the transport node, is pretty straightforward; this is a gateway appliance on which to host this gateway service. The device ID is the bridge (recall that NSX gateway appliances, by default, create OVS bridges to map to their interfaces) connected to the external network.

7. Once you've added two (2) gateway appliances as transport nodes for your gateway service, click Save to create the gateway service and return to NSX Manager. You can create a gateway service with only a single gateway appliance, but it won't be redundant and protected against the failure of the gateway appliance.

NSX is now ready to provide L2 (bridged) connectivity between NSX-backed logical networks and external networks connected to the gateway appliances in the L2 gateway service. Before we can leverage this option inside OpenStack, though, we'll need to first configure OpenStack to recognize and use this new L2 gateway service.

## Configure OpenStack for L2 Connectivity

Configuring OpenStack for L2 connectivity using NSX builds upon the specific details presented in [part 12][p12] of this series. I highly recommend reviewing that post if you haven't already read it.

To configure OpenStack to recognize the L2 gateway service you just created, you'll need to edit the configuration file for the NSX plugin on the Neutron server. In earlier versions of the plugin, this file was called `nvp.ini` and was found in the `/etc/neutron/plugins/nicira` directory. (In fact, this is the information I shared with you in part 12.) Newer versions of the plugin, however, use a configuration file named `nsx.ini` located in the `/etc/neutron/plugins/vmware` directory. I'll assume you are using a newer version of the plugin.

Only a single change is needed to `nsx.ini` in order to configure OpenStack to recognize/use the new L2 gateway service. Simply add the UUID of the L2 gateway service (easily obtained via NSX Manager) to the `nsx.ini` file as the value for the `default_l2_gw_service_uuid` setting. (You followed a similar procedure in part 12 as part of the OpenStack integration, but for L3 connectivity that time.) Then restart the Neutron server, and you should be ready to go!

Neutron recognizes L2 gateway services as _network gateways_, so all the related Neutron commands use the term `net-gateway`. You can verify that the L2 gateway service is recognized by OpenStack Neutron by running the following command with admin permissions:

    neutron net-gateway-list

You should see a single entry in the list, with a description that reads something like "default L2 gateway service" or similar. As long as you see that entry, you're ready to proceed! If you don't see that entry, it's time to check in NSX Manager and/or double-check your typing.

## Adding L2 Connectivity to a Neutron Logical Network

With the NSX gateway appliances installed, the L2 gateway service created, and OpenStack Neutron configured appropriately, you're now in a position to add L2 connectivity to a Neutron logical network. However, there are a few limitations that you'll want to consider:

* A given Neutron logical network may be connected to _either_ a logical router (hosted on gateway appliances that are part of an L3 gateway service) _or_ a network gateway (an L2 gateway service), but not both. In other words, you can provide L3 (routed) _or_ L2 (bridged) connectivity into and out of logical networks, but not both simultaneously.

* Each Neutron logical network may be associated with exactly one broadcast domain on the physical network. Similarly, each broadcast domain on the physical network may be associated with exactly one Neutron logical network. For example, you can't associate VLAN 100 with both logical network A as well as logical network B.

* Finally, by default network gateway operations are restricted to users with administrative credentials only. There is a model whereby tenants can have their own network gateways, but for the purposes of this article we'll assume the default model of provider-supplied gateways.

With these considerations in mind, let's walk through what's required to add L2 connectivity to a Neutron logical network.

1. If you don't already have a logical network, create one using the `neutron net-create` command. This can be done with standard tenant credentials.

2. If you had to create the logical network, create a subnet as well using the `neutron subnet-create` command. You can leave DHCP enabled on this Neutron subnet, as the Neutron DHCP server (which is an instance of `dnsmasq` running in a network namespace on a Neutron network node) won't provide addresses to systems on the physical network. However, the logical network and the physical network are going to be sharing an IP address space, so it would probably be a good idea to control the range of addresses using the `--allocation-pool` parameter when creating the subnet. As with creating the network, standard tenant credentials are all that are needed here.

3. You'll need to get the UUID of the network gateway, which you can do with this command: `neutron net-gateway-list | awk '/\ default\ / {print $2}'`. (You can also assign this to an environment variable for use later, if that helps you.) You'll also need the the UUID of the logical network, which you can also store into an environment variable. This command and all subsequent commands require administrative credentials.

4. Attach the logical network to the network gateway using the `neutron net-gateway-connect` command. Assuming that you've stored the UUID of the network gateway in `$GWID` and the UUID for the logical network in `$NID`, then the command you'd use would be `neutron net-gateway-connect $GWID $NID --segmentation_type=flat`. This command _must_ be done by someone with administrative credentials.

5. If you are using multiple VLANs on the outside of the network gateway, then you'd replace `--segmentation_type=flat` with `--segmentation_type=vlan` and adding another parameter, `--segmentation_id=` and the appropriate VLAN ID. For example, if you wanted to bridge the logical network to VLAN 200, then you'd use `segmentation_type=vlan` and `segmentation_id=200`.

6. That's it! You now have your Neutron logical network bridged out to a broadcast domain on the physical network.

If you need to change the mapping between a broadcast domain on the physical network and a Neutron logical network, simply use `neutron net-gateway-disconnect` to disconnect the existing logical network, and then use `neutron net-gateway-connect` to connect a different logical network to the physical network segment.

I hope you've found this post to be useful. The use of L2 gateways offers administrators and operators a new option for network connectivity for tenants in addition to L3 routing. I'll explore additional options for network connectivity in future posts, so stay tuned. In the meantime, feel free to share any comments, thoughts, or corrections in the comments below.

[all]: /learning-nvp-nsx/
[p6]: {{< relref "2013-10-28-learning-nvp-part-6-adding-an-nvp-gateway.md" >}}
[p11]: {{< relref "2014-03-12-learning-nsx-part-11-reviewing-openstack-integration-basics.md" >}}
[p12]: {{< relref "2014-04-25-learning-nsx-part-12-integrating-vmware-nsx-with-openstack.md" >}}
[1]: {{< relref "2013-08-12-introduction-to-networking-part-1-the-basics.md" >}}
[p9]: {{< relref "2014-02-26-learning-nsx-part-9-adding-a-gateway-service.md" >}}
[p15]: {{< relref "2014-07-16-learning-nsx-part-15-nsx-gateways-gateway-services-and-logical-routers.md" >}}
[p16]: {{< relref "2014-10-13-learning-nsx-part-16-routing-to-multiple-external-vlans.md" >}}
