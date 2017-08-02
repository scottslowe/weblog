---
author: slowe
categories: Tutorial
comments: true
date: 2013-08-19T09:28:32Z
slug: learning-nvp-part-3-nvp-manager
tags:
- Networking
- Nicira
- NVP
- Virtualization
title: 'Learning NVP, Part 3: NVP Manager'
url: /2013/08/19/learning-nvp-part-3-nvp-manager/
wordpress_id: 3244
---

Welcome to part 3 of the Learning NVP blog series. In [part 1][1], I provide an overview of the high-level architecture of NVP and discussed the role of the components in broad terms. In [part 2][2], I focused on the NVP controllers, showing you how to build/configure the NVP controllers and establish a controller cluster. In this part, I'm going to turn my attention to NVP Manager, which will allow us to further configure NVP.

## Reviewing the Role of the NVP Manager

NVP, as you may recall, was expressly designed to integrate with a variety of cloud management platforms (CMPs) via a set of northbound REST APIs. Those northbound REST APIs are actually implemented within the NVP controllers---not within NVP Manager. NVP Manager provides a web-based GUI that is used for certain NVP configuration tasks, such as:

* Adding hypervisors

* Adding transport nodes (gateways and service nodes)

* Configuring transport zones

* Troubleshooting and information gathering

NVP Manager is really more about the _configuration_ of NVP and not the _operation_ of NVP. To put it another way, you'd use NVP Manager to manage the components in NVP, like gateways and hypervisors, but the actual use of NVP---creating logical networks, logical routers, etc.---would generally be done from within the CMP via REST API calls to the NVP controllers. That being said, I'll be using NVP Manager to do some of the things that would normally be handled by the CMP, simply because I don't have a CMP instance to use.

Now that you have a better idea of the role of NVP Manager, let's have a look at building and configuring NVP Manager.

## Building NVP Manager

Like the NVP controllers, NVP Manager is distributed as an ISO. In a production environment, you'd use the ISO to burn optical disks that, in turn, are used to build NVP Manager. In production environments today, NVP Manager is installed on a bare metal server, but it is certainly possible to run NVP Manager in a VM (that's what I'll be doing; I'll actually be running it as a VM in an OpenStack cloud).

To build NVP Manager, just boot from the install media and allow the automated installation routine to complete. Like the controllers, NVP Manager runs on Ubuntu Server 12.04, and the installation routine automatically installs all the necessary components. After the reboot, NVP Manager will boot up to a login prompt. Once you login, you'll be dropped into the NVP CLI with an unconfigured NVP Manager instance.

## Configuring NVP Manager

Configuring NVP Manager, like configuring the NVP controllers, is actually pretty straightforward:

1. Set a password for the admin user (optional, but highly recommended).

2. Set the hostname for NVP Manager (also optional, but highly recommended).

3. Assign an IP address to NVP Manager so it can communicate with the NVP controller cluster.

4. Add an NVP controller cluster to NVP Manager.

Let's take a look at the commands needed to accomplish these steps.

First, setting the password for the admin user is done on NVP Manager just like it was on the NVP controllers:

    set user admin password

You'll be prompted to supply the new password, then retype it for confirmation. Next you can set the hostname, again using the same command as on the NVP controllers:

    set hostname <hostname>

As with the NVP controllers, the NVP Manager installation routine automatically installs [Open vSwitch (OVS)](http://openvswitch.org/) and creates a bridge interface for each physical interface. In my virtual NVP Manager instance, I provided only a single interface (recognized as `eth0`), so the installation created a bridge interface named `breth0`. You can assign the IP address to this bridge interface using this command:

    set network interface breth0 ip config static 192.168.1.3 255.255.255.0

Naturally, you'd want to substitute the correct IP address and subnet mask in that command. Now that network connectivity has been established (you can test it with `ping`), you can add DNS and NTP servers with these commands:

    add network dns-server <DNS server IP address>  
    add network ntp-server <NTP server IP address>

Repeat the commands to add multiple DNS and/or NTP servers. Use `remove` instead of `add` in the above commands to remove a DNS or NTP server address (especially useful if you typed in the wrong address).

&lt;aside&gt;One quick note: if you switch the interface from DHCP to static configuration, the DNS settings still show up in the CLI---_but they don't work._ To fix it, use the commands `clear network dns-servers` and `clear network dns-search-domains`, then add those settings back in with the appropriate `add` command. This is a known issue with this release of NVP (3.1.1) and will be addressed in a future release.&lt;/aside&gt;

Once full network connectivity has been established, we can move past the CLI and start using NVP Manager's web interface. The first task we'll need to accomplish is adding the NVP controller cluster we built in [part 2][2]. Here's how to add the NVP controller cluster to NVP Manager:

1. Log into the NVP Manager web GUI.

2. Because there is no controller cluster, NVP Manager will automatically take you to the screen where you can add a cluster. Click Add Cluster.

3. In the Connect to NVP Controller Cluster dialog, supply an IP address of one of the controllers in the cluster along with the appropriate username and password.

4. Click Connect.

5. Supply a name for this controller cluster, along with an (optional) description and contact.

6. The "Automatically Use New IPs" box is checked by default; this tells NVP Manager to add all the IP addresses of this controller cluster as eligible to receive API calls from the NVP Manager. Recall from [part 2][2] that it is possible to configure multiple interfaces on the controllers and designate certain interfaces to handle API calls, manage OVS devices, etc. If your configuration is such that only certain IP addresses should receive API calls, then uncheck this box.

7. The "Export Logical Stats" checkbox allows you to configure the controllers to make this NVP Manager the collector of logical port statistics. Checking this box is normally the recommended setting, and generally no changes to the default settings are needed. This allows you to query (either via the NVP Manager GUI or via the API) for logical port statistics.

8. The "Make Active Cluster" box should be checked.

9. Click Save.

10. Click Use This NVP Manager to configure the controllers to use the NVP Manager as their userlog server. Click Configure.

This should complete the process of adding the controller cluster to NVP Manager.

Now that NVP Manager is connected to the controller cluster, you can click Dashboard in the header across the top of the NVP Manager web GUI and get a screen that looks something like this:

![NVP Manager dashboard](/public/img/nvp-mgr-dashboard.png)

At this point, you now have a 3-node cluster of NVP controllers and an instance of NVP Manager connected to that cluster. The next step, which I'll tackle in the next part of this series, is adding some hypervisors.

Until then, I encourage everyone to post any questions, suggestions, or clarifications in the comments below. Courteous comments are always welcome.

[1]: {{< relref "2013-05-21-learning-nvp-part-1-high-level-architecture.md" >}}
[2]: {{< relref "2013-08-16-learning-nvp-part-2-nvp-controllers.md" >}}
