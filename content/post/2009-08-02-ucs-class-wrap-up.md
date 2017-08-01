---
author: slowe
categories: Information
comments: true
date: 2009-08-02T23:26:10Z
slug: ucs-class-wrap-up
tags:
- Cisco
- ESX
- FCoE
- FibreChannel
- Networking
- Storage
- UCS
- Virtualization
- VMware
title: UCS Class Wrap-Up
url: /2009/08/02/ucs-class-wrap-up/
wordpress_id: 1507
---

Last week's partner boot camp for the Cisco Unified Computing System (UCS) was very helpful. It has really helped me gain a better understanding of the solution, how it works, and its advantages and disadvantages. I'd like to share some random bits of information I gathered during the class here in the hopes that it will serve as a useful add-on to the formal training. I'm sorry the thoughts aren't better organized.

* Although the UCS 6100 fabric interconnects are based on Nexus 5000 technologies, they _are not_ the same. It would be best for you not to compare the two, or you'll find yourself getting confused (I did, at least) because there are some things the Nexus 5000 will do that the fabric interconnects won't do. Granted, some of these differences are the result of design decisions around the UCS, but they are differences nonetheless.

* You'll see the terms "northbound" and "southbound" used extensively throughout UCS documentation. _Northbound traffic_ is traffic headed out of the UCS (out of the UCS 6100 fabric interconnects) to external Ethernet and Fibre Channel networks. _Southbound traffic_ is traffic headed into the UCS (out of the UCS 6100 fabric interconnects to the I/O modules in the chassis). You may also see references to "east-to-west" traffic; this is traffic moving laterally from chassis to chassis within a UCS.

* For a couple of different reasons (reasons I will expand upon in future posts), there is **no** northbound FCoE or FC connectivity out of the UCS 6100 fabric interconnects. This means that you cannot hook your storage directly into the UCS 6100 fabric interconnects. This, in turn, means that purchasing a UCS alone is not a complete solution---customers need supporting infrastructure in order to install a UCS. That supporting infrastructure would include a Fibre Channel fabric and 10Gbps Ethernet ports.

* Continuing the previous thought, this means that---with regard to UCS, at least---my [previous assertion][1] that there is no such thing as an end-to-end FCoE solution is true. (Read my [correction article][2] and you'll see that I qualified the presence of end-to-end FCoE solutions as solutions that did not include UCS.)

* The I/O Modules (IOMs) in the back of each chassis are fabric extenders, not switches. This is analogous to the Nexus 5000-Nexus 2000 relationship. (Again, be careful about the comparisons, though.) You'll see the IOMs occasionally referred to as fabric extenders, or FEXs. As a result, there is no switching functionality in each chassis---all switching takes place within the UCS 6100 fabric interconnects. Some of the implications of this architecture include:

    1. All east-to-west traffic _must_ travel through the fabric interconnects, even for east-to-west traffic between two blades in the same chassis.

    2. When you use the Cisco "Palo" adapter and start creating multiple virtual NICs and/or virtual HBAs, the requirement for all east-to-west traffic applies to each individual vNIC. This means that east-to-west traffic between individual vNIC instances on the same blade must also travel through the fabric interconnects.

    3. This means that in ESX/ESXi environments using hypervisor bypass (VMDirectPath) with Cisco's "Palo" adapter, inter-VM traffic between VMs on the same host must travel through the fabric interconnects. (This is not true if you are using a software switch, including the Nexus 1000V, but rather only when using hypervisor bypass.)

* Each IOM can connect to a single fabric interconnect only. You cannot uplink a single IOM to both fabric interconnects. For full redundancy, then, you must have both fabric interconnects **and** both IOMs in each and every chassis.

* Each 10Gbps port on a blade connects to a single IOM. To use both ports on a mezzanine adapter, you must have both IOMs in the chassis; to have both IOMs in the chassis, you must have both fabric interconnects. This makes the initial cost much higher (because you have to buy everything), but incremental cost much lower.

* If you want to use FCoE, you must purchase the Cisco "Menlo" adapter. This will provide both a virtual NIC (vNIC) and a virtual HBA (vHBA) for each IOM populated in the chassis (i.e., populate the chassis with a single IOM and you get one vNIC and one vHBA, use two IOMs and get two vNICs and two vHBAs).

* If you use the Cisco "Oplin" adapter, you'll get 10Gbps Ethernet only. There is no FCoE support; you would have to use a software-based FCoE stack.

* The Cisco "Palo" adapter offers the ability to use SR-IOV to present multiple, discrete instances of vNICs and vHBAs. The number of instances is based on the number of uplinks from the IOMs to the fabric interconnects. The formula for calculating this number is 15 * (IOM uplinks) - 2. So, for two uplinks, you could create a total of 28 vNICs or vHBAs (any combination of the two, not 28 each).

* Blades within a UCS are designed to be completely stateless; the full identity of the system can be assigned dynamically using a service profile. However, to take full advantage of this statelessness, organizations will also have to use boot-from-SAN. This further echoes the need for organizations to dramatically re-architect in order to really exploit the value of UCS.

* There are Linux kernels embedded everywhere: in the blades' firmware, in the firmware of the IOMs, in the chassis, and in the fabric interconnects. On the blades, this embedded Linux version is referred to as pnuOS. (At the moment, I can't recall what it stands for. Sorry.)

* In order to reconfigure a blade, the UCS Manager boots into pnuOS, reconfigures the blade, and then boots "normally." While this is kind of cool, it also makes the reconfiguration of a blade take a lot longer than I expected. Frankly, I was a bit disappointed at the time it took to associate or de-associate a service profile to a blade.

* To monitor the status of a service profile association or de-association, you'll use the FSM (Finite State Machine) tab within UCS Manager.

* You'll need a separate block of IP addresses, presumably on a separate VLAN, for each blade. These addresses are the management addresses for the blades. Cisco folks won't like this analogy, but consider these the equivalent of Enclosure Bay IP Addressing (EBIPA) in the HP c7000 environment.

* The UCS Manager software is written in Java. Need I say anything further?

* UCS Manager uses the idea of a "service profile" to control the entire identity of the server. However, admins must be careful when creating and associating service profiles. A service profile that has two vNICs assigned would require a blade in a chassis with two IOMs connected to two fabric interconnects, and that service profile would fail to associate to a blade in a chassis with only a single IOM. Similarly, a service profile that defines both vNICs and vHBAs (assuming the presence of the "Menlo" or "Palo" adapters) would fail to associate to a blade with an "Oplin" adapter because the "Oplin" adapter doesn't provide vHBA functionality. The onus is upon the administrator to ensure that the service profile is properly configured for the hardware. Once again, I was disappointed that the system was not more resilient in this regard.

* Each service profile can be associated to exactly one blade, and each blade may be associated to exactly one service profile. To apply the same type of configuration to multiple blades, you would have to use a service profile template to create multiple, identical service profiles. However, a change to one of those service profiles will not affect any of the other service profiles cloned from the same template.

* UCS Manager does offer role-based access control (RBAC), which means that different groups within the organization _can_ be assigned different roles: the network group can manage networking, the storage group can manage the SAN aspects, and the server admins can manage the servers. This effectively addresses the concerns of some opponents that UCS places the network team in control.

* While UCS supports some operating systems on the bare metal, it really was designed with virtualization in mind. ESX 4.0.0 (supposedly) installs out of the box, although I have yet to actually try that myself. The "Palo" adapter is built for VMDirectPath; in fact, Cisco makes a big deal about hypervisor bypass (that's a topic I'll address in a future post). With that in mind, some of the drawbacks---such as how long it takes to associate or de-associate a blade---become a little less pertinent.

I guess that about does it for now. I'll update this post with more information as I recall/remember it over the next few days. I also encourage other readers who have attended similar UCS events to share any additional points in the comments below.

[1]: {{< relref "2009-07-29-no-such-thing-as-an-end-to-end-fcoe-solution.md" >}}
[2]: {{< relref "2009-07-30-correction-there-might-be-an-end-to-end-fcoe-solution.md" >}}
