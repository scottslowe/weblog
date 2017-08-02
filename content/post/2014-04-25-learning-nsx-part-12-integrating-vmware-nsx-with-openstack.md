---
author: slowe
categories: Tutorial
comments: true
date: 2014-04-25T09:00:00Z
slug: learning-nsx-part-12-integrating-vmware-nsx-with-openstack
tags:
- Networking
- Neutron
- NSX
- NVP
- OpenStack
- Virtualization
- VMware
title: 'Learning NSX, Part 12: Integrating VMware NSX with OpenStack'
url: /2014/04/25/learning-nsx-part-12-integrating-vmware-nsx-with-openstack/
wordpress_id: 3437
---

Welcome to part 12 of the Learning NSX blog series, in which I continue the discussion around integration between [OpenStack](http://openstack.org/) and VMware NSX, and in which I'll provide more details about how exactly to integrate them.

If you are just now joining the series, I encourage you to visit [the Learning NVP/NSX page][1], where you can find links to all the posts in the series. While you'll want to be caught up on all the posts (they do build on one another in various ways), in particular you'll want to make sure you've read part 11. Part 11 covers [the basics of VMware NSX-OpenStack integration][2], and explains how the various components of OpenStack Neutron and VMware NSX will interact.

Once you understand how the different components of Neutron and NSX will interact, getting NSX integrated into OpenStack Neutron isn't too terribly difficult. The basic steps look like this:

1. Install the VMware NSX plugin for Neutron.

2. Configure VMware NSX for Neutron.

3. Configure Neutron for VMware NSX.

Let's take a more in-depth look at each of these steps.

# Installing the VMware NSX plugin for Neutron

VMware distributes a set of compiled binary packages for OpenStack Neutron plus the VMware NSX plugin from the VMware NSX support portal (available to VMware NSX customers). Source code is also available, if you'd prefer that. These builds provided by VMware represent the latest fixes to both Neutron and NSX based off the "official" OpenStack Neutron releases. A single download contains all the different components of Neutron that you need (it's a tarred and gzipped file that you just unpack).

Once you have the packages (I'll assume you're using Ubuntu and therefore have downloaded and will use the Debian packages), then you can just use `dpkg -i` to install the appropriate package(s) on the appropriate node(s). Recall from [part 11][2] that when implementing Neutron with VMware NSX, you'll need both a Neutron server as well as a network node running the DHCP and metadata agents. Here's a breakdown of which packages need to be installed on which nodes:

* On the Neutron server, you'd install `neutron-common`, `neutron-server`, `python-neutron`, and `neutron-plugin-nicira`.

* On the Neutron network node, you'd install (at a minimum) `neutron-common`, `neutron-dhcp-agent`, and `neutron-metadata-agent`. If you wanted LBaaS support, you'd also install `neutron-lbaas-agent`. You could optionally install the Python client with `python-neutron` as well.

From here, you would proceed with setting up OpenStack Neutron as outlined in a variety of places, including [the official OpenStack docs](http://docs.openstack.org/havana/install-guide/install/apt/content/neutron-install-network-node.html). If you do choose to use the official docs to get Neutron configured, here's how the breakdown of the instructions map to the setup you'd need to build for use with VMware NSX:

* The "Installing networking support on a dedicated controller node" section contains information for setting up Neutron on an OpenStack controller that does not run any of the underlying agents. Typically, this system would also run the API servers for some of the other OpenStack services as well (like Nova, Cinder, or Glance).

* The "Installing networking support on a dedicated network node" section contains the information for setting up a network node that would run the DHCP and metadata agents. Recall that you don't need the L3 agent, since that is handled by NSX. It might include the LBaaS agent, if you need that functionality.

* The "Installing networking support on a dedicated compute node" section has the information for setting up your OpenStack compute nodes to interact with Neutron appropriately. Note that you _don't_ need to install an agent on the compute nodes; adding the compute nodes to NSX (as described in [part 4][3]) establishes the necessary communication between the NSX controllers and the compute nodes.

This helps you get Neutron up and running; in the "Configuring Neutron for VMware NSX" section below, I'll provide additional specifics around how to configure Neutron to communicate with VMware NSX. For now, let's make sure that VMware NSX is ready for Neutron.

# Configuring VMware NSX for Neutron

VMware NSX was designed to be cloud platform-agnostic, so there isn't a whole lot that needs to be done here. There are, however, a few tasks you'll want to make sure you've done inside VMware NSX:

  1. You'll want to ensure that you've added at least one NSX gateway appliance to your installation. ([Part 6][4] describes how to add an NVP/NSX gateway appliance.)

  2. You'll want to ensure that you've added an L3 gateway service, as described in [part 9][5] of the series. The L3 gateway service replaces the L3 agent in OpenStack Neutron and is therefore necessary to provide routed/NAT'd connectivity into or out of logical (tenant) networks. Use NSX Manager to get the UUID of the L3 gateway service you've added; you'll need that when you configure Neutron.

  3. You'll need to make sure you've already created a transport zone, as described in [part 4][3] and explained in greater detail in [part 5][6]. Use NSX Manager to get the UUID of the transport zone that you want Neutron to use when creating overlay networks; you'll need that when configuring the NSX plugin for Neutron.

# Configuring Neutron for VMware NSX

Now, let's get into some nitty gritty specifics on how we configure OpenStack Neutron to interact with VMware NSX.

Most of the configuration is done within the NSX-specific configuration files, but there are two settings in `neutron.conf` on the controller node (where the Neutron API server is running) that you'll want to set:

* You'll absolutely want to set the `core_plugin` value to `neutron.plugins.nicira.NeutronPlugin.NvpPluginV2`. (Note that in future releases of the NSX plugin, the name will change from "nicira" and "Nvp" to "vmware" and "Nsx".)

* You'll probably also want to set `allow_overlapping_ips` to True so that Nova metadata works as you would expect. (I'll have more on that in a moment.)

The bulk of the rest of the configuration is found in `nvp.ini`, which is typically found in the `/etc/neutron/plugins/nicira` directory (the name of this directory will change to `/etc/neutron/plugins/vmware` in the future). Here are the relevant settings that you'll want to configure:

* You'll want to set `nvp_user` and `nvp_password` appropriately for your VMware NSX installation.

* Populate the `nvp_controllers` line with the addresses of the NSX controllers in your environment, in the form "W.X.Y.Z:443". Separate the controllers' IP addresses with commas.

* Place the UUID of the transport zone that you want Neutron to use when creating overlay networks as the value for `default_tz_uuid`.

* Place the UUID of the L3 gateway service that you want Neutron to use when creating logical routers with external gateways as the value for the `default_l3_gw_service_uuid` entry.

* In the `[database]` section, make sure there is an appropriate MySQL connection entry for the Neutron database (assuming you are using MySQL). An example connection entry might look like `mysql://neutron:neutron@192.168.100.1/neutron` (or similar).

* There are a couple different ways to provide Nova metadata to the instances; I prefer using a special metadata access network (I'll likely talk more about that in a future post). To use this configuration, set `metadata_mode` to "access_network" and set `enable_metadata_access_network` to True. (You may also need to set `metadata_dhcp_host_route` to False.)

That should be all the settings you need on the controller node. However, you'll also need to slightly configure the DHCP and metadata agents on the network node:

* In the `dhcp_agent.ini` file, set `enable_isolated_metadata` and `enable_metadata_network` to True. If your Linux distribution supports network namespaces (Ubuntu does), then also set `use_namespaces` to True.

* The metadata agent does not require any special configuration above and beyond what is needed to get Neutron running.

Once you restart all relevant services so that they pick up the new settings, you should have Neutron talking to VMware NSX correctly. To test if everything is working correctly, use the Neutron CLI to create a logical network:

    neutron net-create test-network

In my environment (running OpenStack Havana and NSX 4.0.0), that produced output that looked like this:

![Neutron CLI output](/public/img/neutron-cli-output.png)

If all is working as expected, then you should see a matching logical switch listed in NSX Manager:

[![Logical switches in NSX Manager](/public/img/nsx-manager-logical-switch-small.png)](/public/img/nsx-manager-logical-switch.png)

(Click the image above for a larger version.)

It may be obvious, but you'll note that the ID returned by the `neutron net-create` command matches the UUID listed in NSX Manager. You'll also note that the `os_tid` tag assigned to the logical switch in NSX Manager matches the tenant ID of the tenant who owns the logical switch in OpenStack. Finally, you'll note that the bound transport zone's UUID will match the UUID you specified in `nvp.ini` as I outlined earlier.

That's it---you now have VMware NSX integrated with OpenStack Neutron!

In the next post, I'll revisit the topic of logical networking and logical switches within VMware NSX, something I first discussed fairly early in the series. Once I've reviewed some concepts and established a firm foundation, future posts will take a look at how to take advantage of a very cool feature within VMware NSX: the distributed logical router.

In the meantime, feel free to post any questions, clarifications, or thoughts in the comments below. Please include any vendor affiliations, where applicable; otherwise, all courteous comments are welcome!

[1]: /learning-nvp-nsx/
[2]: {{< relref "2014-03-12-learning-nsx-part-11-reviewing-openstack-integration-basics.md" >}}
[3]: {{< relref "2013-08-22-learning-nvp-part-4-adding-hypervisors-to-nvp.md" >}}
[4]: {{< relref "2013-10-28-learning-nvp-part-6-adding-an-nvp-gateway.md" >}}
[5]: {{< relref "2014-02-26-learning-nsx-part-9-adding-a-gateway-service.md" >}}
[6]: {{< relref "2013-09-06-learning-nvp-part-5-creating-a-logical-network.md" >}}
