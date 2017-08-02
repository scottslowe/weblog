---
author: slowe
categories: Tutorial
comments: true
date: 2011-11-29T09:00:00Z
slug: setting-up-software-fcoe-on-vsphere-5
tags:
- FCoE
- Networking
- Storage
- Virtualization
- vSphere
title: Setting up Software FCoE on vSphere 5
url: /2011/11/29/setting-up-software-fcoe-on-vsphere-5/
wordpress_id: 2482
---

One of the features added in vSphere 5 was a _software FCoE initiator_. This is a feature that has gotten relatively little attention, other than a brief mention here and there. I'm not entirely sure why the software FCoE initiator in vSphere 5 hasn't gotten more attention, but this past week I had the opportunity to work with the software FCoE initiator in a bit more detail. In this post I'm going to describe in a bit more detail how to set up the software FCoE initiator; in future posts, I hope to be able to provide some performance and troubleshooting information.

## Prerequisites

In order to use the software FCoE initiator, you must have a network interface card (NIC) that supports the FCoE offloads. The Intel X520 NICs certainly do; these are the cards that I used in my testing. (Disclaimer: Intel provided me a pair of X520 NICs at no charge to use in this testing.) There might be others, I don't know; the VMware HCL doesn't seem to provide an easy way of identifying those NICs that do support the FCoE offloads vs. those NICs that don't.

If you have a NIC that doesn't support FCoE offloads, you won't even be able to add a software FCoE initiator:

![](/public/img/sw-fcoe-not-enabled.jpg)

If, on the other hand, your NIC _does_ support FCoE offloads, you'll see the option to add a software FCoE initiator, like this:

![](/public/img/sw-fcoe-enabled.jpg)

Obviously, you'll want to be sure that your NIC does support FCoE offloads before continuing.

## Setting Up Networking for Software FCoE

Before you can actually set up a software FCoE initiator, you'll first need to configure your networking appropriately. There is one important piece of information you'll want to be sure to have before you continue: the ID of the VLAN for FCoE traffic.

If you've read [my article on setting up FCoE on a Nexus 5000][1], then you know that---on a Nexus 5000 series switch, anyway---you must map the FCoE VSAN to a VLAN (using the `fcoe vsan XXX` command). You're going to need that VLAN ID, so make sure that you have it. In a dual fabric setup, you're going to have two different VLAN IDs. You'll need both.

Once you have those VLAN IDs, you can then proceed with the networking setup:

1. Create a VMkernel port on your ESXi host. When creating the VMkernel port, when prompted for VLAN ID specify the FCoE VLAN on fabric A.

2. I don't know why (and maybe this will be fixed in a future release), but you'll need to assign an IP address to each VMkernel port. The IP address will _not_ be used, so just pick an address on a subnet that you don't use. (Don't use a subnet that you are using elsewhere on the ESXi host or you could run into IP routing weirdness---remember that the VMkernel uses the _first_ interface it finds for a particular route.)

3. After you've created the VMkernel port, modify the NIC teaming policy to only use the physical uplink that is physically connected to fabric A. This might require a bit of sleuthing and/or using CDP/LLDP data to ensure that you have the right uplink selected.

4. Create a second VMkernel port on your ESXi host, this time specifying the FCoE VLAN ID for fabric B and modifying the NIC teaming policy 
When creating the VMkernel ports, specify the appropriate VLAN IDs---one for fabric A, one for fabric B. Modify the NIC teaming policy to only use the physical uplink connected to fabric B, again using physical tracing and CDP/LLDP data as needed to verify it.

At this point, you should now have two VMkernel ports, each with separate (unused) IP addresses and configured for separate VLAN IDs and separate physical uplinks. The VLAN IDs and the physical uplinks should correspond to the FCoE fabrics (fabric A and fabric B).

## Setting Up the Software FCoE Initiator

With the networking in place, you can actually add the software FCoE initiator using the "Add" button on the Storage Adapters view of the Configuration tab. This opens up the Add Software Adapter dialog box I showed you earlier, where you can select "Add Software FCoE Adapter" and click OK.

That option opens the following dialog box:

![](/public/img/add-sw-fcoe-adapter.jpg)

You'll note that the VLAN ID is 0 and isn't changeable. I couldn't find any way to enable this field, and in my testing it proved unnecessary to change it (it worked anyway). Select the appropriate physical uplink and click OK. You'll do this twice---once for fabric A, and again for fabric B. After you've done this twice, you'll have two software FCoE adapters:

![](/public/img/fcoe-adapters-added.jpg)

For each of these two software FCoE adapters, you'll see a node WWN and a port WWN listed. You can use these values in creating the zones and zonesets (see [here][1] for more information). First, though, you'll want to be sure that the software FCoE adapter is actually talking to the fabric correctly; the best way to do that is to use the `show flogi data` command (on a Nexus 5000; other vendors' switches will use a slightly different command). The outcome of the `show flogi data` command will look something like this:

![](/public/img/sw-fcoe-flogi-results.jpg)

In this screenshot (taken from an SSH session into a Nexus 5010), you can see that a device has logged into `vfc1009` on VSAN 300. If you compare the port name and node name, you'll see that they match one of the software FCoE adapters shown earlier. This is only one of the two fabrics; a matching result was seen from the other fabric, showing that both software FCoE adapters successfully logged into their respective fabrics.

At this point, the rest of the configuration consists of creating the appropriate device aliases (if desired); defining zones and zonesets; and presenting storage from the storage array(s) to the initiator(s). Since these are topics that are fairly well understood and well documented elsewhere, I won't rehash them again here.

In future posts, I hope to be able to do provide some performance and some troubleshooting information. However, it will likely be early 2012 before I have that opportunity. In the meantime, if anyone has any additional information they'd like to share---including any corrections or clarifications---please feel free to add them to the comments below.

[1]: {{< relref "2009-10-25-setting-up-fcoe-on-a-nexus-5000.md" >}}
