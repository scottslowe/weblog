---
author: slowe
categories: Tutorial
comments: true
date: 2010-12-15T09:00:00Z
slug: fcoe-through-a-nexus-to-a-mds-attached-storage-array
tags:
- Cisco
- EMC
- FCoE
- FibreChannel
- Interoperability
- Networking
- SAN
- Storage
title: FCoE through a Nexus to a MDS-Attached Storage Array
url: /2010/12/15/fcoe-through-a-nexus-to-a-mds-attached-storage-array/
wordpress_id: 2177
---

In this post, I want to pull together all the steps necessary to take a Converged Network Adapter (CNA)-equipped server and connect it, using FCoE, to a Fibre Channel-attached storage array. There isn't a whole lot of "net new" information in this post, but rather I'm summarizing previous posts, organizing the information, and showing how these steps relate to each other. I hope that this helps someone understand the "big picture" of how FCoE and Fibre Channel relate to each other and how they interoperate (which, quite frankly, is one of the key factors for the adoption of FCoE).

The steps involved come from an environment with the following components:

* A Dell PowerEdge R610 server running VMware ESXi and containing an Emulex CNA

* A Cisco Nexus 5010 switch running NX-OS 4.2(1)N1(1).

* A Cisco MDS 9134 Fibre Channel switch running NX-OS 5.0(1a).

* An older EMC CX3-based array with Fibre Channel ports in the storage processors.

We'll start at the edge (the host) and work our way to the storage. All these steps assume that you've already taken care of the physical cabling.

1. Depending upon how old the software is on your hosts, you might need to install updated drivers for your CNA, as I described [here][1]. If you're using newer versions of software, the drivers will most likely work just fine out of the box.

2. The closest piece to the edge is the FCoE configuration on the Nexus 5010 switch. Here's [how to setup FCoE on a Nexus 5000][2]. Be sure that you map VLANs correctly to VSANs; for every VSAN that needs to be accessible from the FCoE-attached hosts, you'll need a matching VLAN. Further, as pointed out [here][3], the VLAN and VLAN trunking configuration is critical to making FCoE work properly anyway.

3. The next step is connecting the Nexus 5010 to the MDS 9134 Fibre Channel switch. Read this to see [how to configure NPV on a Nexus 5000][4] if you are going to use NPV mode (and read this for more information on [NPV and NPIV][5]). Using NPV or not, you'll also need to setup connections between the Nexus and the MDS; here's [how to setup SAN port channels between a Cisco Nexus and a Cisco MDS][6].

4. Once the Nexus and the MDS are connected, then you'll need to perform the necessary zoning so that the hosts can see the storage. Before starting the zoning, you might find it helpful to [set up device aliases][7]. After your device aliases are defined, you can create the zones and zonesets. This post has information on [how to create zones via CLI][8]; this post has information on [how to manage zones via CLI][9].

At this point---if everything is working correctly---then you are done and you should be ready to present storage to the end hosts.

I hope this helps put the steps involved (all of which I've already written about) in the right order and in the right relationship to each other. If there are any questions, clarifications, or suggestions, please feel free to speak up in the comments.

[1]: {{< relref "2009-08-13-getting-qlogic-gen2-cnas-working-with-esx-4.md" >}}
[2]: {{< relref "2009-10-25-setting-up-fcoe-on-a-nexus-5000.md" >}}
[3]: {{< relref "2009-11-03-fcoe-and-vlan-trunking-on-nexus-5000.md" >}}
[4]: {{< relref "2010-06-28-how-to-configure-npv-on-a-nexus-5000.md" >}}
[5]: {{< relref "2009-11-27-understanding-npiv-and-npv.md" >}}
[6]: {{< relref "2010-11-30-san-port-channels-from-nexus-5010-to-mds-9134.md" >}}
[7]: {{< relref "2010-12-08-using-device-aliases-on-a-cisco-mds.md" >}}
[8]: {{< relref "2009-08-24-new-users-guide-to-configuring-cisco-mds-zones-via-cli.md" >}}
[9]: {{< relref "2009-10-20-new-users-guide-to-managing-cisco-mds-zones-via-cli.md" >}}
