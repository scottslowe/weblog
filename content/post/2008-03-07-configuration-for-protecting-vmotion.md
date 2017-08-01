---
author: slowe
categories: Explanation
comments: true
date: 2008-03-07T10:05:32Z
slug: configuration-for-protecting-vmotion
tags:
- Cisco
- ESX
- IOS
- Networking
- Security
- Virtualization
- VLAN
- VMotion
- VMware
title: Configuration for Protecting VMotion
url: /2008/03/07/configuration-for-protecting-vmotion/
wordpress_id: 653
---

As a follow-up to my earlier post about [using VLANs with VMotion][1], I wanted to also share a brief configuration snippet (based on a Cisco IOS-based switch) that aligns with the recommendations in that article. This is nothing new to those who are familiar with IOS, but for those readers who are not it may provide some helpful information.

For the purposes of this configuration, I'm making the following assumptions:

* VLANs 100 and 200 are the VMotion VLAN and some other production VLAN, respectively (perhaps a VLAN carrying virtual machine traffic, or a VLAN carrying Service Console traffic)

* VLAN 4094 is the "ESX Trunk" VLAN, which isn't used anywhere else in the network for any purpose

Here's the recommended port configuration for ports connecting to an ESX Server:

	interface g0/1  
	switchport trunk encapsulation dot1q  
	switchport trunk native vlan 4094  
	switchport mode trunk  
	switchport trunk allowed vlan 100,200,4094  
	switchport noneg  
	spanning-tree portfast-trunk

Technically, the `switchport noneg` command won't really do anything; it disables DTP (Dynamic Trunking Protocol) but DTP isn't supported by ESX Server anyway.

A couple of notes about this configuration:

* Refer back to my article on [the native VLAN with ESX Server][2]; by setting the native VLAN to 4094 (the "ESX Trunk" VLAN), you won't carry that traffic into the ESX Server. If used on a vSwitch that also carried Service Console traffic, then it could impact automated build scripts.

* If you needed to combine this configuration with EtherChannel, refer back to my original article on [ESX Server, NIC teaming, and VLAN trunking][3].

Keep in mind the other recommendations as well: explicitly control trunking to other devices and explicitly control the transmission of VLANs across trunks to control "VLAN leakage".

CCIEs and other experts, I'd welcome any other suggestions as well as recommended commands to use in the switch configuration to help maximize security and minimize exposure to VLAN-based attacks.

[1]: {{< relref "2008-03-05-vmotion-and-vlan-security.md" >}}
[2]: {{< relref "2007-11-13-esx-server-and-the-native-vlan.md" >}}
[3]: {{< relref "2006-12-04-esx-server-nic-teaming-and-vlan-trunking.md" >}}
