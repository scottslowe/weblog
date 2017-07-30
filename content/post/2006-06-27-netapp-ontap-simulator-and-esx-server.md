---
author: slowe
categories: Explanation
comments: true
date: 2006-06-27T09:07:23Z
slug: netapp-ontap-simulator-and-esx-server
tags:
- ESX
- Hardware
- Interoperability
- Linux
- NAS
- NetApp
- Networking
- ONTAP
- Storage
- Virtualization
- VMware
title: NetApp ONTAP Simulator and ESX Server
url: /2006/06/27/netapp-ontap-simulator-and-esx-server/
wordpress_id: 280
---

In preparation for some [NetApp](http://www.netapp.com/) training that I'll be attending next month, I downloaded the NetApp ONTAP Simulator. The ONTAP Simulator runs on top of Linux (a few different distributions are supported) and allows you to simulate a NetApp Filer. This is pretty cool for a couple of reasons, not the least of which is that it allows you to perform testing of NAS and iSCSI operations without having an actual Filer. Unfortunately, I had some problems getting the ONTAP Simulator working in a virtual machine on [VMware ESX Server](http://www.vmware.com/products/esx/).

To setup the ONTAP Simulator, I created a Linux virtual machine on ESX Server 2.5.3 and installed [Red Hat](http://www.redhat.com/) Linux 9.0. Red Hat 9.0 is a supported distribution for the ONTAP Simulator as well as a fully supported guest OS on ESX Server, and so I didn't expect any issues. However, after installing and configuring the simulator, I couldn't get any network connectivity whatsoever. I had full connectivity to the guest OS, but not to the simulator.

Finally, after digging around in the documentation for the simulator, I came across a statement indicating that the network interface that was being used by the simulator had to be in promiscuous mode. That rang a bell: ESX Server, by default, doesn't allow NICs in guest operating systems to be in promiscuous mode.

The fix is this:

	echo PromiscuousAllowed yes > /proc/vmware/net/vmnic0/config

Replace `vmnic0` in this command with whatever virtual switch or NIC team the virtual machine in question is using. Once I did this (from the Service Console on the ESX Server) and rebooted the virtual machine running the ONTAP Simulator, it worked like a champ.

(Note: You must be a current NetApp customer or partner in order to use the ONTAP Simulator.)
