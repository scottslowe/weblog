---
author: slowe
categories: Tutorial
comments: true
date: 2010-11-30T09:56:59Z
slug: san-port-channels-from-nexus-5010-to-mds-9134
tags:
- Networking
- Cisco
- FibreChannel
- Nexus
- SAN
- Storage
title: SAN Port Channels from Nexus 5010 to MDS 9134
url: /2010/11/30/san-port-channels-from-nexus-5010-to-mds-9134/
wordpress_id: 2165
---

As part of an ongoing effort to expand the functionality of the vSpecialist lab in the EMC RTP facility, we recently added a pair of Cisco MDS 9134 Fibre Channel switches. These Fibre Channel switches are connected to a pair of Cisco Nexus 5010 switches, which handle Unified Fabric connections from a collection of CNA-equipped servers. To connect the Nexus switches to the MDS switches, we used SAN port channels to bond multiple Fibre Channel interfaces together for both redundancy and increased aggregate throughput. Here is how to configure SAN port channels to connect a Cisco Nexus switch to a Cisco MDS switch.

If you are interested, more in-depth information can be found [here](http://www.cisco.com/en/US/docs/switches/datacenter/nexus5000/sw/san_switching/Cisco_Nexus_5000_Series_NX-OS_SAN_Switching_Configuration_Guide_chapter7.html) on Cisco's web site.

Although I've broken out the configuration for the MDS and the Nexus into separate sections, the commands are **very** similar. In my situation, the MDS 9134 was running NX-OS 5.0(1a) and the Nexus 5010 was running NX-OS 4.2(1)N1(1).

## Configuring the Cisco MDS 9134

To configure the MDS 9134 with a SAN port channel, use the following commands.

First, create the SAN port channel with the `interface port-channel` command, like this:

	mds(config)# interface port-channel 1

You can replace the "1" at the end of that command with any number from 1 to 256; it's just the numeric identifier for that SAN port channel. The SAN port channel number does not have to match on both ends.

Once you've created the SAN port channel, then add individual interfaces with the `channel-group` command:

	mds(config)# interface fc1/16  
	mds(config-if)# channel-group 1

The "1" specified in the `channel-group` command has to match the number specified in the earlier `interface port-channel` command. This might seem obvious, but I wanted to point it out nevertheless.

Repeat this process for each interface you want to add to the SAN port channel. In my example, I used two interfaces.

When you add an interface to the SAN port channel, NX-OS reminds you to perform a matching configuration on the switch at the other end, then use the `no shutdown` command to make the interfaces (and the SAN port channel) active. Let's look first at the commands for configuring the Nexus, then we'll examine what it looks like when we bring the SAN port channel online.

## Configuring the Cisco Nexus 5010

The commands here are **very** similar to the MDS 9134. First, you need to create the SAN port channel using the `interface san-port-channel` command (note the slight difference in commands between the MDS and the Nexus here):

	nexus(config)# interface san-port-channel 1

As with the MDS, the number at the end simply serves as a unique identifier for the SAN port channel and can range from 1 to 256.

Then add interfaces to the SAN port channel using the `channel-group` command:

	nexus(config)# interface fc2/1  
	nexus(config-if)# channel-group 1  
	nexus(config-if)# interface fc2/2  
	nexus(config-if)# channel-group 1

As I've shown above, simply repeat the process for each interface you want to add to the SAN port channel. As on the MDS, NX-OS reminds you to perform a matching configuration on the opposite end of the link and then issue the `no shutdown` command.

## Bringing Up the SAN Port Channel

Once a matching configuration is performed on both ends, then you can use the `no shutdown` command (which you can abbreviate to simply `no shut`) to activate the interfaces and the SAN port channel. After activating the interfaces, a `show interface port-channel` (on the MDS) or a `show interface san-port-channel` (on the Nexus) will show you the status of the SAN port channel. Only the first few lines of output are shown below (this output is taken from the Nexus):

	nexus# sh int san-port-channel 1  
	san-port-channel 1 is trunking (Not all VSANs UP on the trunk)  
	Hardware is Fibre Channel  
	Port WWN is 24:01:00:05:9b:7b:0c:80  
	Admin port mode is auto, trunk mode is on  
	snmp link state traps are enabled  
	Port mode is TE  
	Port vsan is 1  
	Speed is 4 Gbps  
	Trunk vsans (admin allowed and active) (1)  
	Trunk vsans (up) ()  
	Trunk vsans (isolated) ()  
	Trunk vsans (initializing) (1)

A few useful pieces of information are available here:

* First, you can see that the SAN port channel is not fully up; it's still initializing. This is shown by the "Not all VSANs UP on the trunk" message, as well as by the "Trunk vsans (initializing)" line.

* Second, you can see the only a single member is up. Note the speed of the SAN port channel is listed as 4 Gbps.

* Third, note that this is a trunking port, meaning that it could carry multiple VSANs. This is noted by the "Port mode is TE" line as well as the first line of the output ("san-port-channel 1 is trunking").

As it turns out, I'd cabled the connections wrong; after I fixed the connections and gave the SAN port channel a small amount of time to initialize, the output was different (this output is taken from the MDS):

	nexus# sh int port-channel 1  
	port-channel 1 is trunking  
	Hardware is Fibre Channel  
	Port WWN is 24:01:00:05:73:a7:72:00  
	Admin port mode is auto, trunk mode is on  
	snmp link state traps are enabled  
	Port mode is TE  
	Port vsan is 1  
	Speed is 8 Gbps  
	Trunk vsans (admin allowed and active) (1)  
	Trunk vsans (up) (1)  
	Trunk vsans (isolated) ()  
	Trunk vsans (initializing) ()

Now you can see that both members of the SAN port channel are active ("Speed is 8 Gbps") and that all VSANs are trunking across the SAN port channel.

At this point, you are now ready to proceed with creating VSANs, zones, and zonesets. Refer to these articles for more information on MDS zone creation and management via CLI:

[New User's Guide to Configuring Cisco MDS Zones via CLI][1]  

[New User's Guide to Managing Cisco MDS Zones via CLI][2]

As always, questions, clarifications, or corrections are welcome---just add them below in the comments. Thanks!

[1]: {{< relref "2009-08-24-new-users-guide-to-configuring-cisco-mds-zones-via-cli.md" >}}
[2]: {{< relref "2009-10-20-new-users-guide-to-managing-cisco-mds-zones-via-cli.md" >}}
