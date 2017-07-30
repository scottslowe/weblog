---
author: slowe
categories: Review
comments: true
date: 2008-08-26T20:12:11Z
slug: first-look-altor-networks-vnsa
tags:
- ESX
- ESXi
- Networking
- Security
- Virtualization
- VMware
title: 'First Look: Altor Networks VNSA'
url: /2008/08/26/first-look-altor-networks-vnsa/
wordpress_id: 843
---

[Altor Networks](http://www.altornetworks.com/) is one of several companies that has emerged from stealth mode over the last six months or so to pitch their products for virtualization security. Altor Networks' product, the Virtual Network Security Analyzer, or VNSA, is their flagship product, but it's important to understand what the VNSA is and what it attempts to do. This product isn't intended to provide network security through enforcement or policy control, but instead is intended to enhance network security through visibility.

When organizations move large numbers of their servers into a virtualized environment, they often lose network visibility in that virtualized environment. This can, in theory, create a security problem because traffic between VMs can't be monitored. One compromised VM could lead to another compromised VM, etc., because the traffic isn't visible to existing security solutions.

&lt;aside&gt;Now, not being a security expert, I have to ask myself: is such a scenario _really_ that likely? It seems to my uneducated mind far too likely that some sort of traffic would "leak" out onto the physical network and be detected there. Or am I just completely misinformed? Anyway, I digress...&lt;/aside&gt;

The VNSA has a two-tier architecture:

* Each ESX server hosting VMs whose traffic should be monitored must have an Altor Agent VM, a small virtual appliance that can monitor up to three separate vSwitches. (This limitation, by the way, is an ESX limitation; VMs are limited to four virtual NICs. Three NICs can be used for monitoring, and the fourth NIC is used for reporting and management.) The Altor Agent relies upon the use of a dedicated port group on each vSwitch that allows promiscuous mode and uses VLAN ID 4095. VLAN ID 4095 is the "special" number that tells ESX to pass traffic from all VLANs up to the guest VM.

* The various Altor Agents report back to a central VM, known as the Altor Center. This is another virtual appliance that is designed to accept the information recorded and reported by the agents. Altor Center also integrates with VMware VirtualCenter to retrieve the list of virtual machines available on the various hosts.

Altor Center provides a web-based interface to view the information gathered and reported by the agents. There are a variety of different reports available, and Altor Center can show traffic patterns between groups of VMs. Users can drill down to see traffic patterns from a variety of perspectives.

While VNSA does a good job of providing network visibility, it does not provide network traffic enforcement functionality. You can't knock the VNSA for failing to enforce security policies considering that _it wasn't intended to do so._ Altor has publicly stated that future products will build upon the foundation established in the VNSA and those future products will provide security policy enforcement. This distinction is important because otherwise users may be inclined to compare virtual security solutions with different feature sets and different target purposes. Comparing the VNSA with a product that is essentially a firewall is not a valid comparison. As a user, if you are looking for a solution to provide visibility into the virtual network environment, VNSA is one solution. If you are seeking security policy enforcement, however, you will need to look elsewhere. VNSA was not intended to fill that need.
