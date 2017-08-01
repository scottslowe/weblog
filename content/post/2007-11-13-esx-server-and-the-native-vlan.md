---
author: slowe
categories: Explanation
comments: true
date: 2007-11-13T16:11:12Z
slug: esx-server-and-the-native-vlan
tags:
- ESX
- Networking
- Virtualization
- VLAN
- VMware
title: ESX Server and the Native VLAN
url: /2007/11/13/esx-server-and-the-native-vlan/
wordpress_id: 574
---

It was December 2006 when I first published this article on [using NIC teams and VLANs with ESX Server][1]. As you can see in the "Top Posts" section in the sidebar, that article has since claimed the top position in the most popular post here on this blog. Note that "most popular" does not translate into "most commented"; that distinction falls to one of the Linux-AD integration articles, although I not sure [which][2] [one][3] right at the moment.

In that previous article, I demonstrated the use of a "dummy VLAN" which was set as the native VLAN for the VLAN trunk, like so:

	s3(config)#int gi0/23  
	s3(config-if)#switchport trunk encapsulation dot1q  
	s3(config-if)#switchport trunk allowed vlan all  
	s3(config-if)#switchport mode trunk  
	s3(config-if)#switchport trunk native vlan 4094

The idea behind the dummy VLAN was this: because ESX Server needed---or so I thought---all the traffic to be tagged as it came across the VLAN trunk, creating a VLAN that is never used and setting it as the native VLAN solves our problem. Remember that the native VLAN is the VLAN whose traffic _is not_ tagged as it travels across the trunk into ESX Server or another physical switch.

It turns out that I was actually mistaken---sort of. It's true that the native VLAN won't get tagged, yes; however, it's not true that ESX Server requires all the traffic to be tagged. What was missing in my configuration was, quite simply, a port group that was intended to receive untagged traffic.

Configuring ESX Server to support VLANs involves the creation of one or more port groups configured with matching VLAN IDs. If a port group has a VLAN ID, it will essentially only accept traffic tagged with that VLAN ID. Traffic not tagged with that VLAN ID, or untagged traffic, will be ignored. So, if you create a series of port groups on a vSwitch for your various VLANs but neglect to create a port group that does not have a VLAN ID specified, untagged traffic will be ignored because there are no port groups configured to receive untagged traffic.

If, on the other hand, you create a series of port groups for your various VLANs _and_ you create a port group that does not have a VLAN ID specified, then both tagged and untagged traffic will be handled correctly:

* Tagged traffic with a VLAN ID matching one of the configured port groups will be sent to that port group

* Tagged traffic with a VLAN ID not matching any configured port group will be ignored

* Untagged traffic will be directed to the port group that does not have a VLAN ID configured

Now, it is true that VMware's best practices documents (sorry, I don't have a link for them at the moment) recommend that users avoid the use of the native VLAN, and one of the CCIEs in my office indicated that it is also considered a networking best practice to avoid the use of VLAN 1, the default native VLAN on Cisco equipment, for anything other than switch management traffic. With those things in mind, it may not be an issue for many deployments.

Except...

...when using automated scripts to build and install ESX Server. You see, after ESX Server is installed, then specifying a VLAN ID on the Service Console port group is no big deal and it will work just fine, as I described earlier. _Before_ ESX Server is installed, though, there is no VLAN support and no way to specify a VLAN ID. Hence, installations that need to download and install from a FTP server or an NFS mount will fail, because the system won't have any network connectivity. (Everyone understands why, right? If you don't, go back and read the earlier paragraphs again.)

What's the fix here? We come back, full circle, to the idea of the default VLAN and untagged traffic. While the system won't accept any tagged traffic during the install process, it will happily accept untagged traffic during the installation. Therefore, if you set the native VLAN to be the VLAN to which the Service Console should be connected once the installation is complete, then everything should work just fine.

Don't believe me? From the "Show Me" state? Perform this quick test yourself:

1. On a test ESX Server, configure the Service Console port group with a VLAN ID of 0. The `esxcfg-vswitch` command is handy for this.

2. Set the switch port to which the Service Console is physically connected to use a native VLAN different than the VLAN the Service Console was previously using. A VLAN with DHCP present is ideal, as you'll see with the next step.

3. Using the `dhclient` command, try to obtain a DHCP lease. You should get a DHCP lease for whatever subnet matches the default VLAN.

4. Repeat steps 2 and 3 and you should see the DHCP lease follow the native VLAN configuration, i.e., whatever VLAN is set to native will be the VLAN that issues a DHCP address to your Service Console.

Hopefully, this helps clear up some of the misunderstanding and confusion around the use of VLANs, VLAN trunks, port groups, and the native---or untagged---VLAN. Feel free to hit me up in the comments if you have any questions!

[1]: {{< relref "2006-12-04-esx-server-nic-teaming-and-vlan-trunking.md" >}}
[2]: {{< relref "2006-08-08-linux-active-directory-and-windows-server-2003-r2-revisited.md" >}}
[3]: {{< relref "2007-01-15-linux-ad-integration-version-4.md" >}}
