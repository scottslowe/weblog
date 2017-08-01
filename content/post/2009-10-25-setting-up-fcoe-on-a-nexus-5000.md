---
author: slowe
categories: Tutorial
comments: true
date: 2009-10-25T19:34:25Z
slug: setting-up-fcoe-on-a-nexus-5000
tags:
- Cisco
- FCoE
- FibreChannel
- Networking
- Nexus
- Storage
title: Setting Up FCoE on a Nexus 5000
url: /2009/10/25/setting-up-fcoe-on-a-nexus-5000/
wordpress_id: 1706
---

Fibre Channel over Ethernet (FCoE) is receiving a great deal of attention in the media these days. Fortunately, setting up FCoE on a Nexus 5000 series switch from Cisco isn't too terribly complicated, so don't be too concerned about deploying FCoE in your datacenter (assuming it makes sense for your organization). Configuring FCoE basically consists of three major steps:

1. Enable FCoE on the switch.

2. Map a VSAN for FCoE traffic onto a VLAN.

3. Create virtual Fibre Channel interfaces to carry the FCoE traffic.

The first step is incredibly easy. To enable FCoE on the switch, just use this command:

	switch(config)# feature fcoe

The next part of the FCoE configuration is mapping a VSAN to a VLAN. What VSAN should you use? Well, if you are connecting to an existing Fibre Channel fabric, perhaps on a Cisco MDS switch, you'll need to make sure that the VSANs between the Nexus and the MDS are appropriately matched. Otherwise, traffic on one VSAN on the Nexus won't be able to reach devices on another VSAN on the MDS. If there's enough demand, I'll post a quick piece on this step as well.

Note that this FCoE VSAN-to-VLAN mapping is a required step; if you don't do this, the FCoE side of the interfaces won't come up (as you'll see later in this post). Assuming the VSAN is already defined, perform these steps to map the VSAN to a VLAN:

	switch(config)# vlan _XXX_  
	switch(config-vlan)# fcoe vsan _YYY_  
	switch(config-vlan)# exit

Obviously, you'll want to substitute `_XXX_` and `_YYY_` for the correct VLAN and VSAN numbers, respectively.

After you've enabled FCoE and mapped FCoE VSANs onto VLANs, then you are ready to create virtual Fibre Channel (vfc) interfaces. Each physical Nexus port that will carry FCoE traffic must have a corresponding vfc interface. Generally, you will want to create the vfc interface with the same number as the physical interface, although as far as I know you are not required to do so. It just makes management of the interfaces easier. The commands to create a vfc interface look like this (obviously you'll want to replace `_ZZ_` with the appropriate port number):

	switch(config)# interface vfc _ZZ_  
	switch(config-if)# bind interface ethernet 1/_ZZ_  
	switch(config-if)# no shutdown  
	switch(config-if)# exit

At this point the vfc interface is created, but it won't work yet; you'll need to place it into an VSAN that is mapped to an FCoE enabled VLAN. If you don't, the `show interface vfc _<number>_` command will report this (asterisks added to show emphasis):

	vfc13 is down **(VSAN not mapped to an FCoE enabled VLAN)**

As I mentioned earlier, if you haven't mapped the FCoE VSAN onto a VLAN, you won't be able to fix this problem. If you have mapped the FCoE VSAN onto a VLAN, then you only need to assign the vfc interface to the appropriate VSAN with these commands:

	switch(config)# vsan database  
	switch(config-vsan-db)# vsan _<number>_ interface vfc _<number>_  
	switch(config-vsan-db)# exit

At this point, the vfc interface will report up, and you should be able to see the host's connection information with the `show flogi database` command.

From this point---assuming that your storage is attached to a traditional Fibre Channel fabric, which is likely to be the case in the near future---you only need to create zones with the WWNs of the FCoE-attached hosts in order to grant them access to the storage. Refer to my posts on [creating zones][1] and [managing zones][2] on a Cisco MDS for more information on this task.

In my own experience, once FCoE was properly configured on the Nexus 5000 switch, then creating zones and zonesets on the Cisco MDS Fibre Channel switch and creating and masking LUNs on the Fibre Channel-attached storage is very straightforward. This, as has been stated on several previous occasions, is one of the strengths of FCoE: it's compatibility with existing Fibre Channel installations is outstanding.

Feel free to submit any questions or clarifications in the comments below.

[1]: {{< relref "2009-08-24-new-users-guide-to-configuring-cisco-mds-zones-via-cli.md" >}}
[2]: {{< relref "2009-10-20-new-users-guide-to-managing-cisco-mds-zones-via-cli.md" >}}
