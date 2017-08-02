---
author: slowe
categories: Tutorial
comments: true
date: 2014-11-03T09:00:00Z
slug: learning-nsx-part-18-routing-without-network-address-translation
tags:
- Networking
- Neutron
- NSX
- OpenStack
- Virtualization
- VMware
title: 'Learning NSX, Part 18: Routing Without Network Address Translation'
url: /2014/11/03/learning-nsx-part-18-routing-without-network-address-translation/
wordpress_id: 3556
---

This is part 18 of the Learning NSX blog series, in which I talk about using layer 3 (L3) routing with VMware NSX but without network address translation (NAT). This post describes a configuration that offers yet another connectivity option for OpenStack cloud administrators and operators.

In [part 6][p6], I showed you how to add a gateway appliance to your NSX installation. [Part 9][p9] leveraged the gateway appliances to create a L3 gateway service, which---as I explained in [part 15][p15]---provides the functionality for logical routers in OpenStack. (Logical routing was covered in [part 14][p14].) [Part 16][p16] expanded the routing configuration to support multiple external networks. This post expands the options again by showing you how to do logical routing without using network address translation (NAT). Of course, it would probably be helpful to read the entire series; links to all posts can be found on the [Learning NVP/NSX page][all].

As I mentioned, so far you've seen three different external connectivity options:

* Routing (layer 3 connectivity) to a single external network

* Routing (layer 3 connectivity) to multiple external networks using VLANs

* Bridging (layer 2 connectivity) between a logical network and a physical broadcast domain

Both of the routed connectivity options you've seen so far have involved the use of NAT (specifically, source NAT, or SNAT) on the logical router in order to establish outbound connectivity. You've had to use floating IP addresses to establish inbound connectivity to instances. However, NSX is absolutely capable of supporting routed connectivity options that do not involve NAT, allowing cloud administrators and operators to use fully routable IP address spaces inside NSX logical networks.

Assuming that you already have logical routing working, the steps to establish a no-NAT routing configuration are pretty straightforward. Note that because the IP address space used in the logical network must interoperate with the IP addressing scheme used on the public network, this configuration is typically set up by a cloud administrator and not by a tenant.

To set up a no-NAT configuration, follow these steps (all commands are assumed to be run using admin credentials):

1. Store the tenant's ID in an environment variable, as you'll be needing it quite a bit over the next few minutes. You can do that using `keystone tenant-list` and `awk`. For example, if the tenant's name was blue, then you could use `keystone tenant-list | awk '/\ blue\ / {print $2}'` to find the tenant ID and assign it to an environment variable. The following commands will assume you've stored the tenant's ID as `$TID`.

2. Create a logical router and assign it to the tenant. The command to do this is `neutron router-create --tenant-id $TID <Name of router>`. It _might_ be possible to re-use an existing logical router, but you're really much better off just creating another logical router for this purpose.

3. Create a logical network on behalf of the tenant, using `neutron net-create --tenant-id $TID <Name of network>`.

4. Create a subnet and associate it with the new logical network you just created. You can do that with this command: `neutron subnet-create --tenant-id $TID --name <Subnet name> --gateway <IP address for logical router> <Name of network> <CIDR>`.

5. Attach the logical router to the logical network and associated subnet with `neutron router-interface-add <Router ID> <Subnet name>`. The IP address of this interface will be the IP address provided to the `--gateway` parameter when you created the subnet.

6. Attach the logical router to the external network that will serve as its uplink. _This is where you'll tell Neutron and NSX that NAT should not be active._ The command to use is `neutron router-gateway-set --disable-snat <Router ID> <External network ID>`.

You're almost done---you now have a logical network using IP addresses from a subnet that is routable within your larger network without any use of NAT. However, you still need to let the rest of the network know about this new subnet, and as NSX in multi-hypervisor environments doesn't yet support dynamic routing via protocols like OSPF and BGP, then you'll need to do static routing.

To do static routing, you'll need the IP address assigned to the router during step 6 above. `neutron port-list` will show you the list of ports in Neutron; depending on the size of your installation, this list might be quite sizable. This command might help:

    neutron port-list -c id -c fixed_ips -c device_owner | awk '/network:router_gateway/'

This will show you only the ports that are acting as router gateway ports. In large environments, even this list may not be short enough to know for sure which IP address is assigned to the router. For now, though, let's assume that you're able to determine the IP address assigned to the logical router on the external network.

Once you have this address, then it's just a simple matter of adding that route to other network devices. Often, this is some form of `ip route add`; the specific commands and syntax will depend on the network device and are outside the scope of this article. Once you get the IP routing table(s) updated on the other network devices, then you're ready to roll!

I hope you find this article (and this series) helpful. As always, if you have any questions, corrections, clarifications, or concerns, I invite you to speak up in the comments below. All courteous comments are welcome!

[all]: /learning-nvp-nsx/
[p6]: {{< relref "2013-10-28-learning-nvp-part-6-adding-an-nvp-gateway.md" >}}
[p9]: {{< relref "2014-02-26-learning-nsx-part-9-adding-a-gateway-service.md" >}}
[p14]: {{< relref "2014-06-20-learning-nsx-part-14-using-logical-routing.md" >}}
[p15]: {{< relref "2014-07-16-learning-nsx-part-15-nsx-gateways-gateway-services-and-logical-routers.md" >}}
[p16]: {{< relref "2014-10-13-learning-nsx-part-16-routing-to-multiple-external-vlans.md" >}}
