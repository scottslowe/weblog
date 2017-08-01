---
author: slowe
categories: Tutorial
comments: true
date: 2014-02-27T09:10:00Z
slug: learning-nsx-part-10-adding-a-service-node
tags:
- Networking
- Neutron
- NSX
- NVP
- OpenStack
- Virtualization
- VMware
title: 'Learning NSX, Part 10: Adding a Service Node'
url: /2014/02/27/learning-nsx-part-10-adding-a-service-node/
wordpress_id: 3411
---

Welcome to part 10 of the Learning NSX blog series, in which I will walk through adding an NSX service node to your NSX configuration.

In the event you've joined this series mid-way, here's what I've covered thus far:

* In part 1, I provided a [high-level overview of NVP/NSX][1] and its core components.

* In part 2, I showed you how to [build NVP/NSX controllers and configure them into a controller cluster][2].

* In part 3, you saw how to [install and configure NVP/NSX Manager][3], a web-based GUI that you can use to configure certain aspects of NVP/NSX.

* In part 4, I walked you through the process of [adding hypervisors to NVP/NSX][4].

* In part 5, I showed you how to [create a logical network][5] that could be used to connect VMs to each other independent of the underlying physical network topology.

* In part 6, I took you through [adding an NVP/NSX gateway appliance][6] to your environment, setting the stage for you to later add L2/L3 connectivity in and out of your logical networks.

* [Part 7][7] and [part 8][8] focused on the transition of this blog series from NVP to NSX; no substantially new information was shared.

* In [part 9][9], we leveraged the gateway appliance from part 6 to add a layer 3 gateway service that can provide L3 (routed) connectivity into or out of your logical networks.

In this installation of the series, I'll walk you through setting up an NSX service node and adding it to the NSX domain. Before I do that, though, it's probably useful to set some context around the role a service node plays in an NSX environment.

## Reviewing Service Nodes in VMware NSX

VMware NSX offers two different ways of handling BUM (Broadcast, Unknown unicast, and Multicast) traffic:

* NSX can perform _source replication_, which means that each hypervisor is responsible for replicating BUM packets and transmitting them onto the logical network(s). In small environments, this is probably fine.

* NSX can also perform _service node replication_, which---as you probably guessed---uses dedicated service node appliances to offload BUM packet replication and transmission. (Service nodes also play a role in multi-DC deployments with remote gateways, but that's a topic for a different day.)

My environment is pretty small and limited on resources, so I don't really _need_ a service node. However, in the current implementation of the integration between OpenStack Neutron and NSX, it assumes the presence of a service node. There is a workaround (I'll probably blog about that later), but I figured I would just go ahead and add a service node to make things easier.

## Building an NSX Service Node

Like the NSX controllers, the NSX gateways, and NSX Manager, the NSX service node software is distributed as an ISO. To install a service node on a physical server, you'd just burn the ISO to an optical disk and boot the server from the optical disk. From the boot menu, select to perform an automated installation, and in a few minutes you're done.

While it is possible to run a service node as a VM (that's what I'm doing), be aware this isn't a supported configuration. In addition, if you think about it, it's kind of crazy---you're building a VM that runs on a hypervisor to offload packet replication from the hypervisor. Doesn't really make sense, does it?

Once the service node is finished installation, you're ready to configure the service node and then add it to NSX.

## Configuring an NSX Service Node

Like the controllers, the gateway, and NSX Manager, the configuration of an NSX service node is pretty straightforward:

1. Set a password for the admin user (optional, but highly recommended).

2. Set the hostname for the service node (also optional, but recommended as well).

3. Assign IP addresses to the service node.

4. Configure DNS and NTP settings.

Let's take a look at each of these steps.

To set the password for the default admin user, just use this command:

    set user admin password

You'll be prompted to supply the new password, then retype it for confirmation. Easy, right? (And pretty familiar if you've used Linux before.)

Setting the hostname for the service node is equally straightforward:

    set hostname <hostname>

Now you're ready to assign IP addresses to the service node. Note that I said "IP addresses" (plural). This is because the service node needs to have connectivity on the management network (so that it can communicate with the NSX controller cluster) as well as the transport network (so that it can set up tunnels with other transport nodes, like hypervisors and gateways). Use this command to see the network interfaces that are present in the controller:

    show network interfaces

You'll note that for each physical interface in the system, the NSX service node installation procedure created a corresponding bridge (this is actually an OVS bridge). So, for a server that has two interfaces (`eth0` and `eth1`), the installation process will automatically create `breth0` and `breth1`. Generally, you'll want to assign your IP addresses to the bridge interfaces, and not to the physical interfaces.

Let's say that you wanted to assign an IP address to `breth0`, which corresponds to the physical `eth0` interface. You'd use this command:

    set network interface breth0 ip config static 192.168.1.5 255.255.255.0

Naturally, you'd want to substitute the correct IP address and subnet mask in that command. Once the interface is configured, you can use the standard `ping` command to test connectivity (note, though, that you can't use any switches to ping, as they aren't supported by the streamlined NSX appliance CLI). For a service node, you'll want to assign `breth0` an IP address on the management network, and assign `breth1` an IP address on your transport network.

Note that you may also need to add a default route using this command:

    add network route 0.0.0.0 0.0.0.0 <Default gateway IP address>

Assuming connectivity is good, you're ready to add DNS and NTP servers to your configuration. Use these commands:

    add network dns-server <DNS server IP address>  
    add network ntp-server <NTP server IP address>

Repeat these commands as needed to add multiple DNS and/or NTP servers. If you accidentally fat finger an IP address, you can remove the incorrect IP address using the `remove` command, like this:

    remove network dns-server <Incorrect DNS IP address>

Substitute `ntp-server` for `dns-server` in the above command to remove an incorrect NTP server address.

To add a DNS search domain to the service node, use this command:

    add network dns-search-domain <Domain name>

If you are using DHCP and your appliance happened to pick up some settings from the DHCP server, you may need to use the `clear network dns-servers` and/or `clear network routes` command before you can add DNS servers or routes to the service node.

Once you've added IP addresses, DNS servers, NTP servers, and successfully tested connectivity over both the management and transport networks, then you're ready to proceed with adding the service node to NSX.

## Adding Service Nodes to NSX

As with adding a gateway appliance in [part 6][6], you'll use NSX Manager (which you set up in [part 3][3]) to add the new service node to NSX. Once you've logged into the NSX Manager web UI via your browser, you're ready to start the process of adding the service node.

1. From the NSX Manager web UI, click on the Dashboard link across the top. If you've just logged into NSX Manager, you're probably already at the Dashboard and can skip this step.

2. In the Summary of Transport Components box, click the Add button on the row for Service Nodes. This opens the Create Service Node dialog box.

3. In step 1 (Type), the Transport Node Type drop-down should already be set to "Service Node." Click Next.

4. Set the display name and (optionally) add one or more tags to the service node object. Click Next to proceed.

5. Make sure that "Admin Status Enabled" is selected, and leave the other options untouched (unless you know you need to change them). Click Next.

6. On step 4 (Credentials), you'll need the SSL security certificate from the service node. Since you have established network connectivity to the service node, just SSH into the new service node and issue `show switch certificate`. Then copy the output and paste it into the Security Certificate box in NSX Manager. Click Next to continue.

7. The final step in NSX Manager is to add a transport connector. A transport connector tells NSX how transport nodes can communicate over a transport zone (I described transport zones back in [part 5][5]). Click Add Connector, then specify the transport type (which tunneling protocol to use), the transport zone, and the IP address. The IP address you specify should match an IP address you assigned to the service node's interface on the transport network. Click OK, then click Save.

At this point, you'll see the number of registered service nodes increment to 1 (assuming this is the first) in the Summary of Transport Components box in NSX Manager. Active, however, will remain zero until you perform the final step.

The final step is performed back on the service node itself. If you opened an SSH session earlier to get the switch certificate, you can just re-use that connection. On the service node, set up communications with the NSX controller cluster using the command `set switch manager-cluster <W.X.Y.Z>`, where W.X.Y.Z represents the IP address of the one of the controllers in your controller cluster. This IP address should be reachable across the interface on the service node assigned to management traffic (which would typically be `breth0`).

Now go back and refresh the Summary of Transport Components box in NSX Manager, and you should see both Registered and Active Service Nodes set to 1 (again, assuming this is the first).

That's really all there is to it. Given their role, you likely won't have a lot of interaction with the service nodes directly. Still, if you want to use NSX with OpenStack Neutron today, you'll want to have a service node present (and, honestly, if you're using NSX and OpenStack in any sort of production environment, you're probably big enough to want a service node anyway).

As always, feel free to post any questions, comments, thoughts, or ideas below. All courteous comments, with vendor disclosures where applicable, are welcome.

[1]: {{< relref "2013-05-21-learning-nvp-part-1-high-level-architecture.md" >}}
[2]: {{< relref "2013-08-16-learning-nvp-part-2-nvp-controllers.md" >}}
[3]: {{< relref "2013-08-19-learning-nvp-part-3-nvp-manager.md" >}}
[4]: {{< relref "2013-08-22-learning-nvp-part-4-adding-hypervisors-to-nvp.md" >}}
[5]: {{< relref "2013-09-06-learning-nvp-part-5-creating-a-logical-network.md" >}}
[6]: {{< relref "2013-10-28-learning-nvp-part-6-adding-an-nvp-gateway.md" >}}
[7]: {{< relref "2013-11-01-learning-nvp-part-7-handling-the-nvp-to-nsx-transition.md" >}}
[8]: {{< relref "2013-12-05-learning-nvp-part-8-an-update-on-the-nvp-to-nsx-transition.md" >}}
[9]: {{< relref "2014-02-26-learning-nsx-part-9-adding-a-gateway-service.md" >}}
