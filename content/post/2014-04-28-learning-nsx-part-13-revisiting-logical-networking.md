---
author: slowe
categories: Explanation
comments: true
date: 2014-04-28T09:00:00Z
slug: learning-nsx-part-13-revisiting-logical-networking
tags:
- Networking
- Neutron
- NSX
- NVP
- OpenStack
- Virtualization
- VMware
title: 'Learning NSX, Part 13: Revisiting Logical Networking'
url: /2014/04/28/learning-nsx-part-13-revisiting-logical-networking/
wordpress_id: 3441
---

Welcome to part 13 of the Learning NSX blog series, in which I revisit the idea of logical networking with VMware NSX. This is a topic I first discussed in [part 5][p5] of this series, but I want to go back and look at it again, this time from a more practical perspective of what it looks like to use VMware NSX for logical networking in an OpenStack environment.

If you haven't been keeping up with the Learning NVP/NSX series, you'll probably want to go back and catch up. Links to all the articles are found on [my Learning NVP/NSX page][all]. You'll particularly want to be sure that you've read [part 11][p11] and [part 12][p12], which cover the OpenStack integration I'll be leveraging in this post.

To start things off, let's first do a quick recap of what it looks like to _manually_ create a logical network in VMware NSX (all of this is described in [part 5][p5] of the series):

1. Create a logical switch.

2. Add logical switch ports to the newly-created logical switch.

3. Edit the attachment of the logical switch ports to connect a VM's virtual network interface card (NIC).

These three steps will establish a simple logical network within VMware NSX. Of course, this logical network won't have any Dynamic Host Configuration Protocol (DHCP) services, but it will still work (you could manually assign IP address to VMs attached to this logical network).

Now that we have VMware NSX integrated with OpenStack, let's revisit this process to see what it looks like. (I'll assume that you're logged into the OpenStack dashboard and have the necessary permissions to create networks, launch instances, etc.)

First, you'd need to create a network in OpenStack. To do this, it's as simple as selecting Networks > Create Network, then providing a name for the new network (you could also use the `neutron net-create` command as well):

[![Creating a logical network in OpenStack](/public/img/openstack-create-network-small.png)](/public/img/openstack-create-network.png)

To exactly mirror the process I showed you in [part 5][p5]---which did not include DHCP services---you'd need to also go to the Subnet tab and uncheck "Create Subnet" as well as go to the Subnet Detail tab and uncheck "Enable DHCP." Once you unselect those options and click Create, then OpenStack will (through the Neutron plugin for NSX) create a logical switch in NSX. You can pop into NSX Manager to see this:

[![New logical switch in NSX Manager](/public/img/nsx-new-logical-switch-small.png)](/public/img/nsx-new-logical-switch.png)

As I pointed out in [part 12][p12], the UUID and `os_tid` tag on this object in NSX will provide the necessary ties back to the corresponding object in OpenStack.

Now go spin up a new instance and attach that instance to the logical network you just created. What you'll find is that OpenStack will _automatically_ handle the creation of the logical switch ports as well as the attachment of the VM's virtual NIC to the logical switch. This helps underscore how VMware NSX was designed to be used in conjunction with a cloud management/orchestration system like OpenStack. (You can verify that the logical switch port is automatically created using NSX Manager and comparing the number of logical switch ports both before and after launching the new instance.)

Now that we have OpenStack up and running, though, we can create a logical network that _does_ have DHCP services:

1. Use the `neutron net-create` command to create a new logical network. For example, `neutron net-create logical-net-02`.

2. Use `neutron subnet-create` to create a subnet for the new network. The full command would look something like `neutron subnet-create --name logical-subnet-02 logical-net-02 10.1.1.0/24` (you'd want to substitute the appropriate IP subnet range there, naturally).

If you log into NSX Manager, you'll see that a new logical switch (whose name matches the name you gave the logical network above) has been created, and you'll also note that 1 logical switch port is already in use---even though you haven't launched any instances yet! The easiest way to find out what is attached to that port is via the OpenStack dashboard. Once logged into the dashboard, select Networks, then click on the network you just created, and scroll down to the list of Ports. You'll see there that OpenStack has automatically created a logical switch port for the DHCP services associated with the subnet you created above:

[![Ports on a logical network](/public/img/openstack-network-ports-small.png)](/public/img/openstack-network-ports.png)

If you're a command-line freak, you could also get this information from the CLI. To find the subnet associated with the logical network you just created:

    SUBNET_ID=$(neutron subnet-list | awk '/\ logical-net-02\ / {print $2}')

To list all the ports on that subnet:

    neutron port-list | grep $SUBNET_ID

In this case (if you've been following my examples), there is only one port on that subnet, so you can capture the ID of that port in order to get more information about the port:

    PORT_ID=$(neutron port-list | grep $SUBNET_ID | awk '{print $2}')

To list the information associated with that specific port, paying particular attention to the `device_owner` attribute (which should show "network:dhcp"):

    neutron port-show $PORT_ID

If you have been reading along diligently, you'll probably be able to put 2 and 2 together here to realize that the "network:dhcp" port is actually a port on OVS on the network node (which, if you'll recall, is registered as a hypervisor in VMware NSX). If you've _really_ been following my stuff closely, you'll probably also know that the OVS port is connected to a veth pair, which in turn connects to a network namespace where an instance of `dnsmasq` is running. (Want to learn more about network namespaces? See [here][1].)

At this point, you should have a fairly clear understanding of how logical networking functions within an OpenStack environment with VMware NSX. I wanted to take the time to revisit this topic because future posts are going to assume that you understand these basic concepts and interactions as we explore more advanced functionality and more complex networking topologies.

Thanks for reading, and feel free to post any corrections, clarifications, or questions in the comments below.

[all]: /learning-nvp-nsx/
[p5]: {{< relref "2013-09-06-learning-nvp-part-5-creating-a-logical-network.md" >}}
[p11]: {{< relref "2014-03-12-learning-nsx-part-11-reviewing-openstack-integration-basics.md" >}}
[p12]: {{< relref "2014-04-25-learning-nsx-part-12-integrating-vmware-nsx-with-openstack.md" >}}
[1]: {{< relref "2013-09-04-introducing-linux-network-namespaces.md" >}}
