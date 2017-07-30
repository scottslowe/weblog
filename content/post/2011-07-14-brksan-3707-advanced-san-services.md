---
author: slowe
categories: Liveblog
comments: true
date: 2011-07-14T15:08:56Z
slug: brksan-3707-advanced-san-services
tags:
- Cisco
- CiscoLive2011
- FibreChannel
- Networking
- SAN
- Storage
title: 'BRKSAN-3707: Advanced SAN Services'
url: /2011/07/14/brksan-3707-advanced-san-services/
wordpress_id: 2344
---

This is BRKSAN-3707, Advanced SAN Services, presented by Mike Dunn.

According to TIP's Storage Research, the top five storage initiatives are deduplication, technology refresh, tiered storage build-out, archiving, and consolidation.

The three main sections of the presentation are SAN consolidation with virtualization, tiered storage and backup design, and Fibre Channel over Ethernet (FCoE).

The presentation starts with SAN consolidation using virtualization. This is really a discussion of virtual SANs (VSANs), which allow you to consolidate SANs onto the same hardware but still providing logical separation of fabrics. In order to move between VSANs, you need to use Inter-VSAN Routing (IVR).

When is IVR needed? When an initiator in one VSAN needs to talk to a target in another VSAN. IVR maintains isolation while allowing for resource sharing.

A common use for IVR is to provide common SAN services, like a shared tape library. IVR would allow media servers in individual VSANs to talk to a shared tape library in a "common" VSAN.

Setting up IVR involves creating an IVR topology. This means you need to manually define the VSANs that will be used for IVR on each switch (all switches that perform IVR will need identical configuration). After defining the IVR topology, you activate it. Then you create your IVR zones and IVR zoneset, just like creating regular zones and zoneset.

IVR works by creating a virtual domain in each VSAN that represents the other VSANs in the topology. Likewise, it creates virtual devices in each VSAN that represent the devices in the other VSANs. This means that logically the initiator thinks the target is in the same VSAN.

Keep in mind that IVR doesn't perform FC ID translation, so domain IDs have to be unique across all VSANs.

IVR does have a Network Address Translation (NAT) mode (IVR NAT). With IVR NAT, the virtual switch is given a randomly available domain ID; this means that you don't need unique domain IDs across all VSANs. IVR NAT is the preferred method of IVR moving forward.

Some operating systems or devices need persistent FC IDs, so IVR NAT allows for static definitions of domain IDs and FC IDs.

Another use case for IVR is SAN extension. You can use IVR to isolate the "remote site" VSAN from the production VSAN, limiting edge VSAN events to only that VSAN. The recommended configuration uses a transit VSAN that connects the two data centers. This keeps the VSANs in each data center isolated to only that data center. (Think of it like a /30 network between two routers.)

A question was asked about using FCIP and whether IVR is needed in this sort of situation. In this case, IVR would not be necessary.

Mike next launches into a quick review of SAN designs. In a core-edge design, there are core switches where storage is attached and edge switches where hosts are attached. This sort of design generally tops out at about 1,700 devices.

For larger environments, you can use an edge-core-edge design. Storage devices have their own edge switches, as do servers, and the edge-to-edge traffic passes through the core. This sort of design tops out at about 4,200 devices.

That discussion was a lead-in to a discussion of NPV/NPIV. This is a topic I've covered previously, so I didn't take notes on this section.

Mike did share some good information on the maximum number of logins per port (42 in switch mode, 114 in NPV mode---watch this value if you are using nested NPIV, which is the term for NPIV hosts connecting to an NPV mode switch) and logins per switch (MDS 9124/9124e/9134/9148).

After discussing NPV/NPIV, Mike moves on to discuss a feature called FlexAttach. FlexAttach resolves the issue of needing to modify zoning and zonesets when an HBA or server needs to be replaced. Basically, any host connecting to an F-port configured as a FlexAttach port will assume the virtual WWPN assigned to that F-port. This eliminates the need to reconfigure zones or zonesets if you replace the server or HBA connected to that F-port. If you're familiar with the behavior of HP VirtualConnect, this appears to be very similar in behavior. FlexAttach is supported on the MDS platform, but is not supported on the Nexus platform.

(Side question: Is FlexAttach leveraged in UCS for vHBA configurations?)

That wraps up the first section; Mike now moves into a discussion of tiered storage and backup design. In this section he will discuss Data Mobility Manager (DMM), SANTap, and Storage Media Encryption (SME).

To perform data migrations, there are different approaches:

* Server/software-based

* Array-based

* Appliance-based

Each of these approaches has advantages and disadvantages. Cisco's solution is DMM, which is a SAN-based migration solution. DMM does both online and offline data migration, uses FC redirects to allow transparent insertion/removal, and is very fast (4.2TB/hr).

FC Redirect is a target-based mechanism for transparently redirecting traffic to a target. With regard to DMM specifically, when using DMM for data migration, FC redirect is used to redirect traffic to the DMM process itself. DMM then sends the I/Os to both the original (source) and destination locations on the SAN. In this regard, it sounds like DMM is performing a form of write-splitting.

In synchronous mode, when handling I/Os to a "migrated" area of the LUN, writes are mirrored. If I/Os are to a "in process" area, writes are queued temporarily until the region has been migrated. For I/Os to "unmigrated" areas are simply sent directly to the source LUN.

In a dual-fabric configuration, each fabric requires its own DMM. Each DMM can handle multiple VSANs.

DMM can also run in an asynchronous mode. In this mode, DMM uses Modified Region Logs (MRLs) to track changes to the source LUN. Any "dirty region" in the MRL is copied across to the target. There is no write penalty as there is with synchronous mode described earlier.

A question was raised about what happens when the data migration is complete. At that point, you'll halt the I/O on the server, complete the job in DMM, and then rezone the fabric to point your host(s) to the new storage target.

You can use the DMM asynchronous mode to migrate data between data centers as well. To prevent having to span a VSAN to the remote site (generally not recommended), you can add another VSAN (a replication VSAN) and a third MSM (a module in the FC switch that runs DMM) to handle the inter-site traffic.

The 120 day evaluation licenses within NX-OS will enable DMM with full functionality for 120 days.

The presentation next shifts to Storage Media Encryption (SME). SME encrypts media for SAN-attached tapes, VTLs, and disk arrays. It uses AES-256 encryption and it is FIPS 140-2 certified. The solution can use a Cisco key management solution or RSA Key Manager. SME is a licensed feature and is only supported on certain platforms (requires certain modules, i.e., the MSM-18/4 or the SSN16).

SME uses FC redirects to transparently insert itself into the data stream to perform encryption.

Cisco's key management solution, Key Management Center, is part of Cisco Fabric Manager. It handles archiving, replicating, recovering, and purging media keys.

Encrypting disks using SME will be available in NX-OS 5.2(1).

The next topic up is SANTap, which as many readers already know is leveraged by EMC RecoverPoint for heterogeneous storage replication. SANTap is a licensed feature but does not use FC redirects. Instead, SANTap uses a host VSAN and a target VSAN. In the host VSAN, SANTap creates a DVT (Data Virtual Target), which uses the WWPN of the real target port. In the target VSAN, SANTap creates a VI (Virtual Initiator), which uses the WWPN of the real host port. SANTAp issues I/Os (or splits I/Os) from the host to the DVT and passes a copy to an additional fabric-based appliance (i.e., a RecoverPoint appliance).

Mike did not have any information on SANTap support for the SCSI commands used by VMware for the VAAI/VAAIv2 offloads in vSphere 4 and vSphere 5. (Bummer!)

The last section of the presentation was on Fibre Channel over Ethernet (FCoE). The information contained in this section was review and stuff that I've already covered elsewhere.
