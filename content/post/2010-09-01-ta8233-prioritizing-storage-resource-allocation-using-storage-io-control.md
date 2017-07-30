---
author: slowe
categories: Liveblog
comments: true
date: 2010-09-01T16:24:39Z
slug: ta8233-prioritizing-storage-resource-allocation-using-storage-io-control
tags:
- Storage
- Virtualization
- VMware
- vSphere
title: 'TA8233: Prioritizing Storage Resource Allocation Using Storage I/O Control'
url: /2010/09/01/ta8233-prioritizing-storage-resource-allocation-using-storage-io-control/
wordpress_id: 2085
---

This is session TA8233, titled "Prioritizing Storage Resource Allocation in ESX Based Virtual Environments Using Storage I/O Control." The presenters are Ajay Gulati and Chethan Kumar, both of whom are R&D Engineers with VMware.

Storage I/O Control (SIOC) is intended to help even out storage resource allocation to prevent some VMs from using up storage resources and negatively impacting other workloads. SIOC implements I/O shares and I/O limits on datastore objects; reservations are not implemented currently.

To enable SIOC for a datastore, simply open the Properties dialog box for the datastore and check the Enabled setting for SIOC. Once it is enabled, you can adjust the default assignments for I/O shares and/or I/O limits on a per-virtual disk basis. When discussing shares, be sure to remember that shares assign _relative_ priority. To help make working with SIOC easier, VMware has included columns for I/O Shares and I/O Limits on the Virtual Machines tab for a selected datastore.

The presenter next shows an example of using SIOC with IOMeter; the example shows that SIOC does actually implement the 2:1 ratio that was configured on the VMs. The next few slides reinforce this behavior as the presenters walk through examples of environments both without SIOC and with SIOC.

SIOC activates when it detects latency above a threshold for an enabled datastore. When the latency exceeds the threshold value, SIOC kicks in and begins to enforce relative priority based on share assignment. The latency is set to a default value, but it is configurable. Lower values enforce stronger isolation; higher values are better for overall throughput. VMware doesn't use only IOPS or only bandwidth for enforcing SIOC; instead, they use the idea of an array queue slot. Some VMs will re-use slots more quickly (sequential I/Os, for example), others will re-use slots more slowly. This is an area I'm going to explore in more detail.

SIOC checks latency every 4 seconds and adjusts host queue depth accordingly. SIOC also detects when VMs are not using their array queue slots and dynamically redistributes those slots to VMs that are actively issuing I/O requests.

The session ends up with a few recommendations:

* Avoid using different settings on datastores that share the same underlying resources. (I wonder how this impacts the use of disk pools in many modern storage arrays?)

* Avoid external access for SIOC-enabled datastores. Do not share across multiple vCenter Server instances, do not access using older hosts or non-ESX hosts, and don't share across datacenters.

* For SSDs, use 10-15 ms as the suggested congestion threshold. For FC and SAS disks, 20-30 ms is appropriate; use 30-50 ms for SATA disks. For auto-tiered datastores, use the vendor-recommended value or use the value from the slowest storage in the pool.

At this point, the session wrapped up. This was a very interesting session and SIOC is a topic that I definitely plan on exploring in much greater detail in the very near future.
