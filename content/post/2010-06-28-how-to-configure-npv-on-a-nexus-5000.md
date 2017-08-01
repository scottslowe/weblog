---
author: slowe
categories: Tutorial
comments: true
date: 2010-06-28T09:00:00Z
slug: how-to-configure-npv-on-a-nexus-5000
tags:
- Networking
- Cisco
- FCoE
- FibreChannel
- Nexus
- NPIV
- NPV
- Storage
title: How to Configure NPV on a Nexus 5000
url: /2010/06/28/how-to-configure-npv-on-a-nexus-5000/
wordpress_id: 1982
---

Some time ago I posted a "how to" article on [how to configure FCoE on a Nexus 5000][1] switch. At that time, I did not put the Nexus 5000 into NPV mode but rather connected it to a Cisco MDS Fibre Channel switch without using NPV. In this entry, I'd like to follow up on that article and show you how to configure NPV on a Nexus 5000.

If you aren't familiar with NPV (N_Port Virtualization) and how it's different than NPIV (N_Port ID Virtualization), check out this article titled ["Understanding NPIV and NPV"][2]; It should help clear things up. Because NPV makes the Nexus 5000 look like an NPIV-enabled host, one potential use for NPV, as in this case, is when connecting the Nexus 5000 to a non-Cisco Fibre Channel switch. Using NPV helps ease interoperability concerns. In this instance, I'm connecting the Nexus 5000 to a Brocade Fibre Channel switch (actually an EMC Connectrix).

Note that I tested these instructions on a Nexus 5010 using both NX-OS 4.1(3)N2(1) as well as NX-OS 4.2(1)N1(1).

The first step is to enable NPV on the Nexus 5000. As far as I can tell, in order to enable NPV you must first enable FCoE using the `feature` command:

	switch(config)# feature fcoe

This loads various Fibre Channel modules and makes possible other features, including NPV and NPIV. Enabling NPV erases the switch configuration and reboots the switch, so be sure you are connected via a console connection before enabling NPV with the `feature` command:

	switch(config)# feature npv

Immediately after enabling NPV, the Nexus 5000 will reboot (you're warned and given the option to proceed or cancel). The warning indicates that the switch's configuration will be removed, but the minimally-configured switches I used in my testing retained their configuration. Granted, I hadn't performed any Fibre Channel or FCoE configurations yet, so perhaps that's the configuration to which the warning was referring.

Once NPV is enabled on the switch, you can then configure Fibre Channel uplink ports as NP ports (proxy N_ports); these are also referred to as external interfaces. To configure a Fibre Channel port as an NP port, use these commands:

	switch# config t  
	switch(config)# interface fc _slot/port_  
	switch(config-if)# switchport mode np  
	switch(config-if)# no shut  
	switch(config-if)# exit  
	switch(config)# exit

You should then be ready to physically connect to the upstream Fibre Channel switch, which---if you recall correctly from my earlier NPV/NPIV post---needs to be NPIV-enabled. In this particular case, I was uplinking the Nexus 5010 to an EMC-rebranded Brocade switch (a Connectrix DS-300B running Fabric OS 6.1.0). To show whether the port on the Connectrix was enabled for NPIV, I used the `portcfgshow` command:

	rtp-fc-sw-01:admin> portcfgshow <port number>

Look for the line that says "NPIV Capability"; the value should be reported as "ON". If the value is not "ON", you'll need to use the `portcfgnpivport` command to enable NPIV on the specified port, like this:

	rtp-fc-sw-01:admin> portcfgnpivport <port number> 1

The "1" at the end of that command enables NPIV; a "0" would disable NPIV for that port.

Once NPIV is enabled on the upstream Fibre Channel switch, when you physically connect the configured external interface then the Fibre Channel link should come up. I used the `show int fc <slot/number>` command on the Nexus to verify that the port was up; on the Connectrix, I used the `portshow <port>` command. In addition, I was also able to see the Nexus switch logged into the Fibre Channel fabric on the Connectrix using the `nsshow` command.

Once you have Fibre Channel connectivity via the external interfaces, then configuring FCoE to hosts connected to the Nexus follows the same set of instructions laid out in [my earlier FCoE how-to article][1].

From that point on, it's only a matter of configuring zones (see here for [help with zones on a Cisco MDS][3]) and presenting storage. Those are different posts for a different day...

[1]: {{< relref "2009-10-25-setting-up-fcoe-on-a-nexus-5000.md" >}}
[2]: {{< relref "2009-11-27-understanding-npiv-and-npv.md" >}}
[3]: {{< relref "2009-08-24-new-users-guide-to-configuring-cisco-mds-zones-via-cli.md" >}}
