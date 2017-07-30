---
author: slowe
categories: Explanation
comments: true
date: 2008-03-11T12:51:22Z
slug: identifying-esx-server-nics-in-blades
tags:
- Cisco
- ESX
- Hardware
- Networking
- Virtualization
- VMware
title: Identifying ESX Server NICs in Blades
url: /2008/03/11/identifying-esx-server-nics-in-blades/
wordpress_id: 656
---

This is a handy little trick that many of you may already know. Starting with version 3.5, VMware added support for Cisco Discovery Protocol (CDP) on the ESX Server vSwitches. CDP support is enabled on a vSwitch with this command:

	esxcfg-vswitch -B both vSwitch0

The "-B" parameter is case-sensitive; the "-b" (note the lowercase B) displays CDP status while the "-B" (uppercase B) configures CDP.

Once CDP support is enabled on the vSwitch---and assuming it is enabled on the physical switch---running the `show cdp neighbor` IOS command will show the link between each physical switch port and the matching ESX Server NIC. The output will look something like this:
    
    Capability Codes: R-Router, T-Trans Bridge, B-Source Route Bridge
                      S-Switch, H-Host, I-IGMP, r-Repeater, P-Phone
    
    Device ID Local Intrfce Holdtme Capability Platform      Port ID
    s3        Gig 0/26        147      T S     WS-C3524-XFas 0/24
    esx04     Gig 0/22        168       S      VMware        ESXvmnic0
    esx04     Gig 0/21        168       S      VMware        ESXvmnic1

As you can see in the output above, the CDP output clearly links the physical switch port and the ESX Server NIC. This makes it incredibly easy to identify the NICs in the server. This is particularly helpful in blade situations, since you can't exactly unplug the NIC and see which one goes down with `esxcfg-nics -l` (a common approach to identifying the NICs in the server). Of course, this requires Cisco switches in the blade chassis. Since the internal port mappings on the blade chassis determine which NICs connect to which ports, this command adds the mapping within ESX Server and lets us quickly and definitively identify the NICs in the server as seen by ESX Server.
