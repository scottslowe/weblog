---
author: slowe
categories: Explanation
comments: true
date: 2008-10-08T16:03:36Z
slug: more-on-vmware-esx-nic-utilization
tags:
- Cisco
- ESX
- IOS
- Networking
- Virtualization
- VLAN
- VMware
title: More on VMware ESX NIC Utilization
url: /2008/10/08/more-on-vmware-esx-nic-utilization/
wordpress_id: 978
---

Earlier this year, I wrote an article about [NIC utilization in VMware ESX][1] in which I discussed some of the details around how VMware ESX uses NICs in various configurations. The testing that prompted that article was primarily centered around IP-based storage (software-based iSCSI and NFS), so naturally the discussion primarily centered around IP-based storage as well.

As a follow up to that article, I've been running some additional tests with a focus this time on the behavior of virtual machines (VMs) in configurations both with and without link aggregation. The results are very consistent with the findings from the previous set of tests, but with a few key items to keep in consideration when applied specifically to VMs.

## Without Link Aggregation

As stated in the earlier article, using NIC teaming without link aggregation means the vSwitch is configured with one of these three load balancing settings:

1. Route based on the originating virtual port ID

2. Route based on source MAC hash

3. Use explicit failover order

The primary focus here is "Route based on the originating virtual port ID," since that's the default vSwitch setting and the recommended setting from VMware when not using link aggregation.

Just as all the VMware docs explain, the tests show that a VM will use only one uplink, regardless of how many different connections are involved, and it will stay on that uplink until the uplink fails, at which time the VM will be moved to a different uplink according to the NIC failover order configured for that port group or vSwitch.

There are, however, a few other things to consider:

* There is no guarantee that the VMs will be evenly distributed across the uplinks on a vSwitch or port group. Although I've seen references in the VMware documentation to indicate that VMs are balanced in some fashion (I could not find those references, however), real-world experience seems to indicate otherwise. In my tests, I had instances where four VMs were all "linked" (via their virtual port ID) to the same uplink.

* It's unclear at what point VMware ESX creates the link between the virtual port ID and the uplink. In my tests, I rebooted the guest OS and power cycled the VM only to have it come back on the same uplink again. Only a VMotion off the server and back again caused a VM to move to a new uplink. I've been trying to track down more information on the timing of the association between the virtual port ID and the uplink, but have thus far been unsuccessful.

* There is no control over the placement of VMs onto uplinks without the use of multiple port groups. (Keep in mind that you can have multiple port groups corresponding to a single VLAN.)

* Because multiple VMs could be assigned to the same uplink, and because users have no control over the placement of VMs onto uplinks, it's quite possible for multiple VMs to be assigned to the same uplink, or for VMs to distributed unevenly across the uplinks. Organizations that will have multiple "network heavy hitters" on the same host and vSwitch may run into situations where those systems are all sharing the same network bandwidth.

These considerations are not significant, but they are not insignificant, either. The workaround for the potential problems outlined above involves using multiple port groups with different NIC failover orders so as to have more fine-grained control over the placement of VMs on uplinks. In larger environments, however, this quickly becomes unwieldy. The final release of the Distributed vSwitch will help ease configuration management in this sort of situation, but that's still out in the future yet.

## VM Networking with Link Aggregation

Using NIC teaming with link aggregation means that the vSwitch is configured with "Route based on ip hash" and that the physical switch has been configured for EtherChannel or static 802.3ad/LACP.

&lt;aside&gt;By the way, static 802.3ad/LACP in the Cisco IOS world means the use of the `channel-group X mode on` command as opposed to `channel-group X mode active`; the first command uses static link aggregation whereas the second uses dynamic link aggregation. Static is supported; dynamic is not.&lt;/aside&gt;

In this environment, the tests showed exactly what we expected; traffic from VMs was distributed across the uplinks in a dynamic fashion based upon the source and destination IP addresses. While some of these things may be common sense, there are a few things to consider:

* Depending upon the number of uplinks, it's certainly possible that some traffic streams may end up getting hashed onto the same uplink. One a traffic stream is placed onto an uplink, it won't move off that uplink until the flow is complete.

* The way in which outbound traffic from the VMs will be placed onto the uplinks won't necessarily be the same as the way the physical switch places inbound traffic onto the uplinks. A good resource on how Cisco handles placing traffic onto members of an EtherChannel bundle is [this page](http://www.cisco.com/en/US/tech/tk389/tk213/technologies_tech_note09186a0080094714.shtml).

* Multiple connections to or from a VM _might_ utilize multiple uplinks but aren't necessarily guaranteed to use multiple uplinks.

Again, some of the points above are simple common sense, but they should be kept in mind nevertheless. If the workloads that are being hosted on VMware ESX are such that having more aggregate bandwidth would be beneficial, then using link aggregation is really the only way to do it. I suppose it would be possible to use NIC teaming inside the guest OS, assuming the VM was configured to use e1000 NICs, but I've never tested this configuration. (Anyone else tested it?)

[1]: {{< relref "2008-07-16-understanding-nic-utilization-in-vmware-esx.md" >}}
