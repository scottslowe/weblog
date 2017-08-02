---
author: slowe
categories: Tutorial
comments: true
date: 2013-10-28T09:00:00Z
slug: learning-nvp-part-6-adding-an-nvp-gateway
tags:
- Networking
- Nicira
- NVP
- Virtualization
- VMware
title: 'Learning NVP, Part 6: Adding an NVP Gateway'
url: /2013/10/28/learning-nvp-part-6-adding-an-nvp-gateway/
wordpress_id: 3319
---

Welcome to part 6 of the Learning NVP blog series. In this part, I'm going to show you how to add an NVP gateway appliance to your NVP environment. In future posts, you'll use this NVP gateway to host either L2 or L3 gateway services (more on those in a moment). First, though, let's take a quick recap of what's transpired so far:

  * In part 1, I provided a [high-level overview of NVP][1] and its core components.

  * In part 2, I showed you how to [build NVP controllers and configure them into a controller cluster][2].

  * In part 3, you saw how to [install and configure NVP Manager][3], a web-based GUI that you can use to configure certain aspects of NVP.

  * In part 4, I walked you through the process of [adding hypervisors to NVP][4].

  * In part 5, I showed you how to [create a logical network][5] that could be used to connect VMs to each other independent of the underlying physical network topology.

In this part, I'm going to walk you through setting up an NVP gateway appliance. If you'll recall from our [introductory high-level architecture overview][1], the role of the gateway is to provide L2 (switched/bridged) and L3 (routed) connectivity between logical networks and physical networks. So, adding a gateway would then enable you to extend the [logical network you created in part 5][5] to include either L2 or L3 connectivity to the outside world.

&lt;aside&gt;Many of you have probably seen some of the announcements from VMworld about NSX integrations from various networking suppliers (Arista, Brocade, Dell, and Juniper, for example). These announcements will allow NSX---which I've said before will leverage a great deal of NVP's architecture---to use these hardware devices as L2 gateways, providing bridged/switched connectivity between logical networks and physical networks.&lt;/aside&gt;

This post will focus only on getting the gateway appliance set up; in future posts, I'll show you how to actually add the L2 or L3 connectivity to your logical network.

## Building the NVP Gateway

The NVP gateway software is distributed as an ISO, like the NVP controller software. You'd typically install this software on a bare metal server, though with recent releases of NVP it is supported to install the gateway into a VM (refer to the latest NVP release notes for more details). As with the NVP controllers and NVP Manager, the gateway is built on Ubuntu 12.04, and the installer process is completely automated. Once you boot from the ISO, the installation will proceed automatically; when completed, you'll be left at the login prompt.

## Configuring the NVP Gateway

Once the NVP gateway software is installed, configuring the gateway is really straightforward. In fact, it feels a lot like configuring NVP controllers (I suspect this is by design). Here are the steps:

1. Set the password for the admin user (optional, but highly recommended).

2. Set the hostname for the gateway appliance (also optional, but strongly recommended).

3. Configure the network interfaces; you'll need management, transport, and external connectivity. (I'll explain those in more detail later.)

4. Configure DNS and NTP settings.

Let's take a closer look at these steps. The first step is to set the password for the admin user, which you can accomplish with this command:

    set user admin password

From here, you can proceed with setting the hostname for the gateway:

    set hostname <hostname>

(So far, these commands should be pretty familiar. They are the same commands used when you set up the NVP controllers and NVP Manager.)

The next step is configure network connectivity; you'll start by listing the available network interfaces with this command:

    show network interfaces

As you've seen with the other NVP appliances, the NVP gateway software builds an Open vSwitch (OVS) bridge for each physical interface. In the case of a gateway, you'll need at least three interfaces---a management interface, a transport network interface, and an external network interface. The diagram below provides a bit more context around how these interfaces are used:

![NVP gateway appliance interfaces](/public/img/nvp-gw-interfaces.png)

Since these interfaces have very different responsibilities, it's important that you properly configure them. Otherwise, things won't work as expected. Take the time to identify which interface listed in the `show network interfaces` output corrsponds to each function. You'll first want to establish management connectivity, so that should be the first interface to configure. Assuming that `breth0` (the bridge matching the physical `eth0` interface) is your management interface, you'll configure it using this command:

    set network interface breth0 ip config static 192.168.1.12 255.255.255.0

You'll want to repeat this command for the other interfaces in the gateway, assigning appropriate IP addresses to each of them.

You may also need to configure the routing for the gateway. Check the routing table(s) with this command:

    show network routes

If there is no default route, you can set one using this command:

    add network route 0.0.0.0 0.0.0.0 <Default gateway IP address>

Once the appropriate network connectivity has been established, then you can proceed with the next step: adding DNS and NTP servers. Here are the commands for this step:

    add network dns-server <DNS server IP address>  
    add network ntp-server <NTP server IP address>

If you accidentally fat-finger an IP address or hostname along the way, use the `remove network dns-server` or `remove network ntp-server` command to remove the incorrect entry, then re-add it correctly with the commands above.

Congrats! The NVP gateway appliance is now up and running. You're ready to add it to NVP. Once it's added to NVP, you'll be able to use the gateway appliance to add _gateway services_ to your logical networks.

## Adding the Gateway to NVP

To add the new gateway appliance to NVP, you'll use NVP Manager (I showed you how to set up NVP Manager in [part 3][3] of the series). Once you've opened a web browser, navigated to the NVP Manager web UI, and logged in, then you can start the process of adding the gateway to NVP.

1. Once you're logged into NVP Manager, click on the Dashboard link across the top. (If you're already at the Dashboard, you can skip this step.)

2. In the Summary of Transport Components box, click the Connect & Add Transport Node button. This will open the Connect to Transport Node dialog box.

3. Supply the management IP address of the gateway appliance, along with the appropriate username and password, then click Connect.

4. After a moment, the Connect to Transport Node dialog box will show details of the gateway appliance, such as the interfaces, the bridges, the NIC bonds (if any), and the gateway's SSL certificate. Click Configure at the bottom of the dialog box to continue.

5. Supply a display name (something like nvp-gw-01) and, optionally, one or more tags. Click Next.

6. Unless you know you need to select any of the options on the next screen (I'll try to cover them in a later blog post), just click Next.

7. On the final screen, you'll need to establish connectivity to a transport zone. You'll want to select the appropriate interface (in my example environment, it was `breth2`) and the appropriate encapsulation protocol (STT is generally recommended for connectivity back to hypervisors). Then select the appropriate transport zone from the drop-down list. In the end, you'll have a screen that looks something like this (note that your interfaces, IP addresses, and transport zone information will likely be different):

	[![Adding a gateway to NVP](/public/img/add-nvp-gateway-small.png)](/public/img/add-nvp-gateway-fullsize.png)

8. Click Save to finish the process. The number of gateways listed in the Summary of Transport Components box should increment by 1 in the Registered column. However, the Active column will remain unchanged---that's because there's one more step needed.

9. Back on the gateway appliance itself, run the command `set switch manager-cluster <NVP controller IP address>` (you can use the IP address of any controller in the NVP controller cluster).

10. Back in NVP Manager, refresh the Summary of Transport Components box (there's a small refresh icon in the corner), and you'll see the Active column update to show the gateway appliance is now registered _and_ active in NVP.

That's it---you're all done adding a gateway appliance to NVP. In future posts, you'll leverage the gateway appliance to add L2 (bridged) and L3 (routed) connectivity in and out of logical networks. First, though, I'll need to address the transition from NVP to NSX, so look for that coming soon. In the meantime, feel free to post any questions, thoughts, or suggestions in the comments below. I welcome all courteous comments (even if you disagree with something I've said!).

[1]: {{< relref "2013-05-21-learning-nvp-part-1-high-level-architecture.md" >}}
[2]: {{< relref "2013-08-16-learning-nvp-part-2-nvp-controllers.md" >}}
[3]: {{< relref "2013-08-19-learning-nvp-part-3-nvp-manager.md" >}}
[4]: {{< relref "2013-08-22-learning-nvp-part-4-adding-hypervisors-to-nvp.md" >}}
[5]: {{< relref "2013-09-06-learning-nvp-part-5-creating-a-logical-network.md" >}}
