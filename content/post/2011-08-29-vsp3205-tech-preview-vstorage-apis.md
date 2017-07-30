---
author: slowe
categories: Liveblog
comments: true
date: 2011-08-29T13:52:24Z
slug: vsp3205-tech-preview-vstorage-apis
tags:
- Virtualization
- VMware
- VMworld2011
- vSphere
- Storage
title: 'VSP3205: Tech Preview, vStorage APIs'
url: /2011/08/29/vsp3205-tech-preview-vstorage-apis/
wordpress_id: 2381
---

This is a session blog for VSP3205, titled "Tech Preview: vStorage APIs for VM and Application Granular Data Management." The presenters are Satyam Vaghani and Vijay Ramachandran, both with VMware.

This session will be repeated tomorrow at 1PM and Wednesday (didn't catch the time). As usual, the presentation starts out with the VMware disclaimer, followed by a quick review of the "state of the union" with vStorage APIs for Array Integration (VAAI) and vStorage APIs for Storage Awareness (VASA). Both of these features enable greater communication and information exchange between vSphere and the underlying storage arrays. They are attempts to "bridge the gap" between vSphere's logical view and the array's physical view. While these features do help, there is still more to be done. What is really needed is a general framework to leverage future storage array features and enhancements.

The presenters share some quotes and comments from customers, where the feedback primarily resolves around greater granularity (i.e., being able to failover a single VM, or offer differentiated services on a per-VM/per-application basis). The existing VAAI and VASA features aren't granular enough to meet these requests/demands from customers. VMware needs more granular data management.

The reason VMware can't provide more granular data management currently is because vSphere is managing VMs and VMDKs but the arrays are operating at LUNs or RAID groups or volumes. This again reinforces the need for a larger framework that allows more integration with arrays and at the same time offers granular data management.

Here are the ideal "wish list" requirements:

* Ability for VMware to offload per VMDK-level operations to storage systems

* Build a framework for future features and enhancements

* No disruption to existing VM creation workflows

* It needs to be highly scalable

Next we see the VMware vision. They want to move to an application-centric view of the world. Let the applications specify the policies (the requirements), and let the virtualization and storage layers provision against those policies. In addition, the unit of management should be the same between vSphere and the array. That is, the unit of management needs to be the VMDK.

RDMs can help accomplish some of these goals, but the management overhead is tremendous.

Now Satyam takes the stage to show VMware's technology direction. Satyam reinforces the disconnect between vSphere (which operates at/on the virtual storage layer/fabric) and the arrays (which operate at/on the physical storage layer/fabric). The physical and virtual fabrics never exchange information, which means that information like QoS and hardware-based data services cannot be effectively leveraged.

So the goal is to enable storage systems to natively storage VMDKs as distinct entities and provide granular VMDK data services. This interaction would be done via a policy-based interface where vSphere acts as an arbitrator of services between the virtual fabric and the physical fabric. This functionality requires new storage access methods and new vStorage APIs, and it will fundamentally change how storage is provisioned and managed in vSphere environments.

VMware decided that VMFS was not a good option for storage system differentiation and new storage solutions. Likewise, NAS was not an ideal solution, because most data services are not at file granularity. Object-based storage was considered, but VMware felt the protocol (part of T10) was not successful.

Satyam next shows a demonstration of a _VM volume_, which is a representation of a VMDK on a storage system. The demonstration was using a future build of ESXi and a future build of an EMC storage array with VM volume awareness. This demo shows how, for the first time, the virtualization layer and the storage layer working on the same management objects. It operates the same across both SAN and NAS. Lots of questions arise from this demonstration: Does this mean millions of VM volumes? Are we re-inventing storage systems? Is this even feasible?

To accomplish the goal of enabling VM volumes, VMware has the idea of a _IO Demultiplexer,_ or IO Demux. The IO Demux is a special I/O channel from the host to the entire storage system. Behind the IO Demux will reside thousands of VM volumes. To handle the capacity management issue---i.e., preventing the vSphere administrator from creating too many VM volumes and overrunning the entire array---VMware introduces the idea of the _capacity pool_ (CP). The capacity pool is not visible in the data path; it is not a LUN or mount point. CPs can span arrays, or could even span datacenters. The VM admin/user can carve VM volumes out of the CP until they run out of space. (However, this doesn't address IOPs requirements for VM volumes or the CP. How will IOPs requirements be handled?)

Profiles enable the application to communicate its specific QoS requirements to the storage system. (Is this how we handle IOPs?) Profiles will have fixed and customizable attributes (snapshots are allowed, snapshot retention is a certain value, snapshot frequency is a certain value, etc.). (Let's hope that these attributes are implemented in a more granular basis than the current VASA implementation.) VM admins/users can customize attributes on a per-VM (i.e., per VM volume) basis.

Satyam next moves into a demonstration with more detail on the IO demultiplexer. In this demo using protocol EMC equipment, we show multiple IO demultiplexers using multiple storage protocols.

Following this demo, Satyam shows a prototype vCenter Server UI from VMware showing capacity pools with various storage capabilities (profiles).

Next there is a demo of prototype storage from NetApp showing the awareness of the capacity pool from vCenter Server and using an NFS-based IO demultiplexer.

From there we move into a CLI-based demonstration of the same capabilities using a Dell EqualLogic array.

The next demonstration flips back into a prototype of EMC Unisphere to show off storage profiles, where the storage administrator has defined multiple storage profiles at the storage array.

Putting everything together, VM volumes looks a lot like profile-driven storage in vSphere 5 today: the storage profiles are defined at the array level (instead of at the vSphere level) and the destination "datastore" is a capacity pool instead of a LUN or NFS mount point. After creating a VM using this process, we flip over to EMC Unisphere to see the individual VM volumes created on the array, and looking at the properties for each VM volume. Satyam also shows demos of the same operation on prototype NetApp and Dell EqualLogic arrays.

The next demonstration shows an IBM XIV prototype that supports VM volumes and shows a VM being cloned at the hardware level, on a per-VM/per-VM volume basis.

The session wraps up with a review of the four major components: IO demultiplexer, capacity pools, storage profiles, and VM volumes. Satyam closes the session with a mention of the partners that participation vendors: Dell, VMware, HP, NetApp, IBM, and EMC.
