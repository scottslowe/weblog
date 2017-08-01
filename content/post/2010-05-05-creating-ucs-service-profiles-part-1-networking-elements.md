---
author: slowe
categories: Tutorial
comments: true
date: 2010-05-05T09:39:23Z
slug: creating-ucs-service-profiles-part-1-networking-elements
tags:
- Cisco
- Hardware
- Networking
- UCS
- VLAN
- Virtualization
title: 'Creating UCS Service Profiles Part 1: Networking Elements'
url: /2010/05/05/creating-ucs-service-profiles-part-1-networking-elements/
wordpress_id: 1916
---

This article is part 1 of a series of posts that provide some additional information on creating UCS service profiles. In this part of the series, I provide some guidelines on networking-related elements that should be created before you create a UCS service profile. By creating these elements before creating the service profiles or service profile template, the process of creating the service profile or service profile template is a bit smoother.

There are three key networking elements that I recommend defining before you create a UCS service profile:

1. At least one MAC address pool

2. All the VLANs that need to be visible/accessible within UCS

3. At least one vNIC template

The steps for creating each of these elements, along with their location in UCSM, is found below.

## Creating a MAC Address Pool

_Location in UCSM: LAN tab - LAN > Pools > MAC Pools_  
_Prerequisites: None_

Here are the steps for creating a MAC address pool:

1. Right-click on MAC Pools and select Create MAC Pool.

2. Specify a name and description for the new MAC pool, then click Next.

3. Click Add (with the green plus symbol) to add a new MAC block.

4. Specify the first MAC address and the size of the MAC block. Keep in mind that MAC pools (and the MAC address blocks within them) are not shared across UCSM instances, so be sure to provide a way of distinguishing MAC addresses across multiple UCSM instances. For example, use the fourth hextet to denote a UCSM instance (01 for the first UCSM instance, 02 for the second, etc.). This will help generate unique MAC addresses across multiple UCSM instances.

5. Click OK to add the MAC address block to the MAC pool.

6. Repeat steps 3 through 5, as needed, to add more MAC address blocks to this MAC pool. When the MAC address pool is complete, click Finish to complete the creation of the new MAC pool.

## Defining VLANs

_Location in UCSM: LAN tab - LAN > LAN Cloud > VLANs_  
_Prerequisites: None_

1. Right-click on VLANs and select Create VLAN(s).

2. Specify a descriptive name for the VLAN.

3. Specify whether the VLAN should be global to both UCS fabrics, specific to only one fabric or the other, or if the fabrics should be configured differently for the same VLAN. Generally, VLANs will be configured as Common/Global.

4. Specify the VLAN ID(s) that are associated with this VLAN object in UCSM. Generally, each VLAN object in UCSM should only have a single VLAN ID associated with it. If you enter multiple VLAN IDs (like "70-75" or "21-24,27"), UCSM will create separate VLAN objects for each VLAN ID specified. This makes the creation of multiple VLANs a single step.

5. When the dialog box is complete, click OK to create the VLAN object.

6. Repeat this process to create VLAN objects for all VLANs that should be available within this UCSM instance. Use multiple VLAN IDs, as outlined earlier, to streamline the creation of multiple VLANs at the same time.

## Creating a vNIC Template

_Location in UCSM: LAN tab - LAN > Policies > vNIC Templates_  
_Prerequisites: MAC address pool, VLANs_

1. Right-click on vNIC Templates and select Create vNIC Template.

2. Specify a name and a description for this vNIC template.

3. Select the UCS fabric to which this vNIC template should be associated. Do not select Enable Failover when using VMware ESX/ESXi. Note that you will need to create vNIC templates for both fabrics in dual fabric configurations.

4. Leave all targets selected.

5. Select Updating to make this an updating template. As additional VLANs are added in the future, modifying this template will then automatically modify service profiles created from this template to make the new VLANs available to associated blades.

6. Place a check mark next to each VLAN that should be visible to vNICs created from this vNIC template.

7. Mark one of the selected VLANs as the native VLAN; traffic from this VLAN will be untagged traffic. (In VMware ESX/ESXi, this traffic would require a port group with a blank VLAN ID configured.)

8. Select the MAC address pool created earlier.

9. If necessary, choose values for QoS policy, network control policy, pin group, and stats threshold policy. In general, these values do not need to be set.

10. When the dialog box is complete, click OK to create the vNIC template.

11. If this is a dual fabric installation, repeat this process for the second UCS fabric.

In [part 2][1] of the series, I'll provide information on the storage-related elements that I recommend creating before you create a UCS service profile.

If you have any questions or clarifications, please feel free to post them in the comments.

[1]: {{< relref "2010-05-07-creating-ucs-service-profiles-part-2-storage-elements.md" >}}
