---
author: slowe
categories: News
comments: true
date: 2011-07-12T13:01:00Z
slug: vmwares-launch-event
tags:
- Security
- Storage
- vCloud
- Virtualization
- VMware
- vSphere
title: VMware's Launch Event
url: /2011/07/12/vmwares-launch-event/
wordpress_id: 2330
---

Today was a big day for VMware. I'm going to provide some summary coverage of the products launched today, but only a quick recap; I'll have more in-depth analysis and information on the products and their key features and improvements in future blog posts. No doubt there is going to be plenty of other coverage on the launch as well, and I'll likely produce a special "Short Takes" episode with a summary of some of the related links, so look for that as well.

Now, on to the product announcements!

## vSphere 5

As fully expected, VMware today announced VMware vSphere 5, the next generation of their virtualization suite. VMware continues to drive virtualization "higher" in the data center as they target even the most mission critical applications, so vSphere 5 offers support for massive VMs (up to 32 vCPUs and 1 TB of RAM per VM). With vSphere 4, there were only a few instances where a mission critical application couldn't be supported because of resource constraints. That already-slim window shrinks even more now with vSphere 5.

Also in the vSphere 5 release, VMware has added a lot of features to help simplify and automate the virtualization layer. This is fully expected and a natural part of vSphere's continued maturation. Some of the features that VMware packed into this release for improved administration and management include:

* vSphere Auto Deploy: VMware now offers a fully supported PXE boot solution that offers completely stateless ESXi hosts. Need to deploy a new ESXi host? No problem, with Auto Deploy it can be done in minutes. Need to deploy a new ESXi image? Change a few rules in the Auto Deploy engine and reboot your host---and you're done. It's pretty powerful stuff, in my opinion.

* Storage DRS: vSphere DRS is the darling of many data centers, transparently moving VMs around to keep cluster workloads balanced. vSphere 5 introduces the same concept for storage, called Storage DRS. (Just as a side note, I'm not clear if the "DRS" in "Storage DRS" still stands for "Distributed Resource Scheduler," since that's not really applicable to storage. Anyone know?) Using information on storage capacity usage and (optionally) I/O response times, Storage DRS can shift virtual disks for VMs from datastore to datastore---using the concept of a datastore cluster---to keep storage utilization balanced. Like vSphere DRS, Storage DRS also performs initial placement, simplifying the VM storage provisioning process. This is something that has been in the works for _years_ (I first heard about it from VMware in 2008), and it's great to see it finally make it's appearance.

* Profile-Driven Storage: This is another killer feature. Building on the vSphere Storage APIs for Storage Awareness (more popularly known as VASA), profile-driven storage allows administrators to define VM storage profiles that describe the features or attributes that storage must possess in order to satisfy the requirements of the VM (RAID type, disk type, capacity, protection level, replication, snapshots, etc.). Then, based on VM storage profiles, when you create a new VM, perform a Storage vMotion, or clone a VM or template, vSphere will use the VM storage profile to show you which datastores are compatible (compliant with the profile) or incompatible (noncompliant with the profile). This provides a huge benefit to the vSphere administrator in ensuring that VMs are stored on the right storage with the right support.

Of course, that's not all that vSphere 5 has to offer; there's also a laundry list of other new features:

* VAAI v2, which includes hardware offloads for NFS and new thin provisioning awareness

* All-new framework for vSphere HA, which eliminates the primary/secondary model and provides significant new features

* A new vSphere Storage Appliance, to turn local (DAS) storage into shared storage in environments where a dedicated SAN isn't possible and performance is not the key consideration

* A new version of VMFS that offers datastores up to 64TB in size without the use of extents

* Significant performance enhancements for Storage vMotion, and the ability to relocate snapshots

* Improvements in NFS to support scale-out NAS

* Software FCoE initiator (only supported on Intel X520 NICs at initial release). I have a couple of the Intel X520 NICs that I'll be doing some additional testing with against vSphere 5, so look for those results on this site soon.

As you can see, it's quite a significant release. But wait, there's more...

## vCloud Director 1.5

VMware also announced vCloud Director 1.5, which offers a number of new features:

* New APIs: vCloud Director will offer broadened "southbound" APIs, so that solutions like vCO, UIM, and others can provide further automation in highly virtualized environments

* Linked Clone support: vCD 1.5 will support linked clones, so that deploying new workloads will happen faster and with less storage consumption.

## Site Recovery Manager 5

In addition to vSphere 5 and vCD 1.5, VMware also unveiled SRM 5, with new features like:

* Built-in automated failback: While vendors such as EMC provided failback plugins, those plugins didn't provide the full functionality of SRM when performing a failback. SRM 5 now provides full failback support, a key feature that many organizations have been requesting.

* Workload mobility workflows: SRM starts adding support for workload mobility workflows, to move workloads cold between sites. (Hmmm...think about VPLEX plus SRM workload mobility workflows...give you any ideas?)

* vSphere host-based replication: vSphere 5 can now offer replication on a host-based level, for environments where array-based replication isn't possible due to constraints (budgetary or otherwise). Naturally, there's a trade-off in using host-based replication versus array-based replication, but it's a nice feature to add for lower-end customers.

## vShield 5

Last, but not least, VMware announced vShield 5. vShield 5 brings improved management to the table as well as some new features:

* Static routing functionality in vShield Edge: This provides vSphere and vCloud Director administrators greater flexibility in modeling network topologies.

* New product offering in the form of vShield Data Security: This is an integration of technology from RSA DLP (Data Loss Prevention), that offers administrators the ability to discover and report sensitive data in virtual machines.

All in all, VMware unveiled a lot of new functionality today that is targeted at driving the further adoption of virtualization and addressing concerns over virtualizing mission critical applications.

As I mentioned earlier, look for more in-depth articles on some of the new features and functionality in the coming days and weeks. Thanks!
