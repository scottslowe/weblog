---
author: slowe
categories: Tutorial
comments: true
date: 2010-05-07T10:26:24Z
slug: creating-ucs-service-profiles-part-2-storage-elements
tags:
- Cisco
- Hardware
- Storage
- UCS
- VSAN
title: 'Creating UCS Service Profiles Part 2: Storage Elements'
url: /2010/05/07/creating-ucs-service-profiles-part-2-storage-elements/
wordpress_id: 1918
---

This article is part 2 of a series of posts that provide some additional inormation on creating UCS service profiles. In [part 1][1] of the series, I discussed some of the networking-related elements that you should create before you create your service profile (or service profile template). In this post, I'm going to discuss the storage-related items that you should create in advance of creating the service profile (or service profile template).

Much like there are three networking-related elements you should define before defining the service profile, there are five storage-related elements that I recommend you define in advance of creating a service profile:

1. At least one WWNN address pool

2. At least one WWPN address pool

3. All the VSANs that need to be visible/accessible within UCS

4. At least one vHBA template

5. A local disk configuration policy

Below I've included the steps for creating each of these elements, along with their location in UCSM and a brief mention of any prerequisites.

## Creating a WWNN Address Pool

_Location in UCSM: SAN tab - SAN > Pools > WWNN Pools_  
_Prerequisites: None_

Here are the steps for creating a WWNN pool:

1. Right-click on WWNN Pools and select Create WWNN Pool.

2. Supply a name and description for this WWNN pool, then click Next to continue.

3. Click Add (with the green plus symbol) to create a new WWNN address block. (This is a brand new pool, so it starts out with no address blocks.)

4. Specify the starting WWNN address. As with MAC addresses, these addresses are not shared or coordinated among multiple UCSM instances, so you should take steps to ensure that WWNN addresses are unique across the entire SAN fabric. For example, you could use the sixth hextext to denote UCSM instance (01 for the first UCSM, 02 for the second UCSM, etc.). Note that Cisco provides a base WWNN address that they strongly recommend you use in order to ensure unique addresses across the SAN fabric.

5. Click OK to add the WWNN address block.

6. Repeat steps 3 through 5 to add more WWNN address blocks as needed. Click Finish to create the WWNN pool.

## Creating a WWPN Address Pool

_Location in UCSM: SAN tab - SAN > Pools > WWPN Pools_  
_Prerequisites: None_

Here are the steps for creating a WWPN address pool:

1. Right-click on WWPN Pools and select Create WWPN Pool.

2. Specify a name and description for the new WWPN pool, then click Next.

3. Click Add (with the green plus symbol) to add a new WWPN block.

4. Specify the starting WWPN address. As with MAC addresses and WWNN addresses, these addresses need to be unique across multiple UCSM instances, so specify the base address accordingly. To ensure uniqueness, it is also possible to specify a different Cisco OUI (for example, to use 20:00:00:25:b4:xx:xx:xx as the base address instead of 20:00:00:25:b5:xx:xx:xx as the base address), but it is unclear what risk this raises of potential SAN address conflicts. [This page](http://standards.ieee.org/regauth/oui/oui.txt) provides a list of registered OUIs that you can use to help ensure unique addresses.

5. Click OK to add the WWPN block.

6. Repeat steps 3 through 5 to add more WWPN address blocks as needed. When you are done, click Finish to complete the creation of the WWPN pool.

## Defining VSANs

_Location in UCSM: SAN tab - SAN > SAN Cloud > VSANs_  
_Prerequisites: None_

Here are the steps to define a VSAN that must be visible/accessible in UCS:

1. Right-click on VSANs and select Create VSAN.

2. Specify a descriptive name for the VSAN.

3. Select whether the VSAN should be global to both fabrics, specific to only a single fabric, or configured differently on each fabric. Generally, when defining VSANs, each VSAN is specific to a particular fabric. Select the appropriate fabric that will host this VSAN.

4. Specify the VSAN ID and the matching FCoE VLAN ID. The FCoE VLAN must have been already defined, and fabrics cannot share the same FCoE VLAN.

5. Click OK to create the VSAN.

6. Repeat this process for all other VSANs that must be visible within this UCSM instance. If this is a dual fabric installation (and all production installations should be dual fabric installations), you will also need to repeat this process to define the VSANs for the second fabric.

## Creating a vHBA Template

_Location in UCSM: SAN tab - SAN > Policies > vHBA Templates_  
_Prerequisites: WWNN pool, WWPN pool, VSANs_

Here are the steps to create a vHBA template:

1. Right-click vHBA Templates and select Create vHBA Template.

2. Supply a name and description for the new vHBA template.

3. Select the fabric to which this vHBA will connect.

4. Select the VSAN to which this vHBA will be associated.

5. Select Updating template so that changes to this vHBA template will propagate to vHBAs created from this template. In general, this is less of an issue with vHBAs than it is for vNICs.

6. Select the previously created WWPN pool. Generally, all other values on this dialog box remain at their default values.

7. Click OK to create the vHBA template.

8. In a dual fabric installation, you must repeat this process to create a vHBA template for the other fabric. If you are using a Cisco Virtual Interface Controller (VIC) mezzanine card, you can define multiple vHBA templates per fabric.

## Creating a Local Disk Configuration Policy

_Location in UCSM: Servers tab - Servers > Policies > root > Local Disk Config Policies_  
_Prerequisites: None_

If you are using organizations (for role-based access control), you might need to define this policy within the specific organization where you want it to be visible. Otherwise, here are the steps for defining a local disk configuration policy:

1. Right-click on Local Disk Config Policies and select Create Local Disk Configuration Policy.

2. Specify a name and description for the new local disk configuration policy.

3. From the drop-down list, select the configuration mode for this policy. When doing boot from SAN, select No Local Storage from the mode drop-down list and ensure that hard drives are not actually installed in blades to which this policy will be applied. This policy won't apply successfully (when incorporated into a service profile) if hard drives are installed in the server. Likewise, selecting a mode that requires hard drives to be installed will fail to apply to blades that don't actually have hard drives installed. You will want to verify that the local disk configuration policy you define will actually apply to the blades that are found within the UCS.

4. When you are ready, click OK to create the local disk configuration policy.

In [part 3][2] of the series, I will cover any other remaining elements that I recommend you define in advance of creating your service profiles or service profile templates. Then, in part 4, I'll bring it all together and discuss the creation of a service profile or service profile template when you have pre-created the elements I've described in this series.

As always, feel free to post questions, clarifications, or corrections in the comments below.

[1]: {{< relref "2010-05-05-creating-ucs-service-profiles-part-1-networking-elements.md" >}}
[2]: {{< relref "2010-05-10-creating-ucs-service-profiles-part-3-other-elements.md" >}}
