---
author: slowe
categories: Liveblog
comments: true
date: 2008-06-11T11:09:33Z
slug: vir360-microsoft-system-center-vmm-2008-part-2-of-2
tags:
- HyperV
- Microsoft
- TechEd2008
- Virtualization
- Windows
title: 'VIR360: Microsoft System Center VMM 2008, Part 2 of 2'
url: /2008/06/11/vir360-microsoft-system-center-vmm-2008-part-2-of-2/
wordpress_id: 732
---

We're getting ready to start VIR360, the second of two sessions this morning on System Center Virtual Machine Manager (SCVMM) 2008. The first session was [liveblogged here][1]. The wireless network in this room isn't nearly as reliable as the signal in the last room, so I may not be able to publish this until after the session ends. We'll see.

This session is being presented by Alan Goodman, Senior Program Manager for Microsoft. The idea behind the presentation is to focus on the advanced features of SCVMM 2008, building upon what was presented earlier in VIR253. The presenter for VIR253, Edwin, alluded to the possibility that this session might be heavily focused on PRO, one of the integration points between SCVMM and System Center Operations Manager (SCOM). I'm hoping that's not the case.

As the session starts, Alan outlines the three areas that he will focus on: VMware management, PRO integration with SCOM, and Hyper-V failover cluster integration.

Alan starts out with VMware management. The reason behind Microsoft tackling VMware management with SCVMM 2008 is in response to customer demand. This allows customers to leverage management processes and the System Center family for a complete management solution. It's also nice to see Microsoft admitting that the co-existence of virtualization platforms is a reality. ESX and Hyper-V will co-exist in customers' data centers, and customers want to be able to manage both with a single console.

As a result of customer demand, some of the goals for SCVMM 2008 were:

* Cross-platform virtualization management (managing VI3, Hyper-V, and Virtual Server)

* Streamlining operations and automating with PowerShell

* Enhanced management of VMware, including PRO, intelligent placement, library support, and integration conversion tools

Alan refers again to using SCVMM as the "manager of managers", bringing together multiple VirtualCenter environments and Microsoft virtualization environments into a single management console. SCVMM uses the existing, published VI3/VirtualCenter interfaces, and operations performed in either tool will reflect in both environments in real-time. A single set of credentials is used between SCVMM and VI3; to fully exploit SCVMM's capabilities, full privileges must be granted to the credentials used by SCVMM.

Alan moves into a demo of VMware management. To add VMware management, you click a link labeled "Add VMware VirtualCenter server" and supply the name of the VC server and supply credentials. From there, ESX hosts will start adding to the SCVMM console, and then VMs will start populating in SCVMM as well. From there, Alan initiates a VMotion operation from within SCVMM. Upon initiating the process inside SCVMM, we see the task appear inside VirtualCenter, reflecting the real-time communication between the two environments.

All this talk about intelligent placement makes me wonder what Microsoft's recommendation or requirement is regarding DRS on managed VMware clusters? Is there a recommendation to disable DRS? Would VC's intelligent placement functionality conflict with the SCVMM intelligent placement capability?

I saw Alan supply some credentials during the SCVMM demo, and it looked like he used root credentials on the ESX host. Is that required? That implies that SCVMM communicates outside of VC, which would contradict some discussions with the SCVMM team yesterday. If anyone can provide any clarification, that would be great.

Alan next moves on to a discussion of SCVMM's failover clustering integration. He does clearly define Quick Migration as a solution that does involve downtime, but he quickly repositions Quick Migration as an HA solution when used in conjunction with Windows Server 2008 failover clustering. Alan makes comparisons between Quick Migration and VMware HA, which in my mind are very applicable comparisons. I'm glad to see Microsoft owning up that Quick Migration is not equivalent to VMotion (live migration).

When placing VMs onto a failover cluster, SCVMM performs various permutations to ensure that the capacity requirements of the new VM don't exceed the cluster's ability to supply those resources in a failure scenario, based on the cluster's configured cluster reserve. For VMware pros, think of this like configuring a VMware HA cluster and admission control. (Let's hope that SCVMM doesn't run into the same kinds of quirks as VC's admission control.)

VMs are configured to be highly available on a per-VM basis, and SCVMM will automatically handle the placement of VMs on HA-configured hosts (either failover clusters with Hyper-V or VMware HA clusters). In some ways, I do like the per-VM ability to configure high availability.

Alan next moved into a more in-depth discussion of Performance and Resource Optimization (PRO), an integration point between SCVMM and SCOM. The integration with SCOM provides a number of reports pertaining to virtualization:

* Virtualization candidates
* Virtual machine allocation
* Virtual machine utilization
* Host utilization
* Host utilization growth

The ability for customers already using SCOM to then leverage the integration between SCOM and SCVMM to drive a P2V/consolidation effort is pretty powerful. It wouldn't make sense for an organization to adopt SCOM just for consolidation assistance, but if an organization is already using SCOM then it's a nice added feature.

The host utilization growth report could also be very useful in helping to determine capacity allocation and to project future resource needs.

SCVMM also leverages an SDK connection to SCOM to provide real-time updates to SCOM as VMs are created, migrated, or decommissioned.

So what is PRO? PRO is workload- and application-aware resource optimization. With PRO, we can create policies that act upon tips, provided by SCOM as part of its OS and application monitoring ability, to address potential resource utilization problems. In some ways, PRO is kind of like VMware DRS, but since Hyper-V doesn't provide any live migration functionality. In that regard, it falls far short of matching the DRS functionality. However, where it exceeds VMware DRS is in more detailed knowledge about the applications and services running inside the VM, instead of acting only upon the "external view" of the VM's resource requirements. This is why I think that the VMware acquisition of B-Hive is critical, because it begins to give VMware the same kind of "application awareness" inside the VM so that DRS can act upon service-level agreements or service-level status.

PRO also provides an extensible framework (assuming via SCOM's management/monitoring capabilities) to allow hardware vendors to supply hardware monitoring information and other software vendors to provide more detailed information and extensions to PRO. Examples include Brocade (presumably to provide Fibre Channel fabric information), Emulex (Fibre Channel HBA information), EMC (storage array performance information), and HP (server hardware information).

Alan goes on to verify that PRO is built upon the idea of Management Packs (MPs) for SCOM. Microsoft provides their own PRO-enabled MP for SCOM that provides information for Hyper-V hosts, VMware hosts, and virtual machines; this MP provides information back to SCVMM in order to enable PRO tips and recommendations inside SCVMM 2008.

Moving into a demo, Alan shows how the feedback between SCVMM and SCOM creates a comprehensive service view that crosses the physical-to-virtual boundary, allowing administrators to drill down into clusters, then into hosts, then into VMs, and then finally into specific services inside VMs. That's pretty handy.

Continuing the demo, Alan shows how PRO can resolve problems for you; he uses the example of a couple of VMs generating high CPU load on the host. By telling PRO to fix the problem, SCVMM's intelligent placement is invoked and a new host is selected for the VM. The VM is then migrated to the new host. The only piece missing: live migration. Because this was done on a Hyper-V host, we had to use Quick Migration, which then caused some downtime for the end users.

At this point, Alan concluded the session and opened the floor to questions.

**UPDATE:** There's some additional information that I need to add to this post as a result of the Q&A portion of the session. In that portion of the session, I asked Alan about two things:

1. Interactions between PRO and DRS when SCVMM is managing VMware hosts

2. Credentials required to "fully" manage VMware ESX servers from SCVMM

In response to the first question, Alan indicated that DRS and PRO can conflict with each other. The recommendation is to disable DRS in the VMware environment and allow PRO to handle the optimization of the cluster. Keep in mind, as mentioned in the comments below and as I mentioned above, that SCVMM can initiate VMotion/live migration operations when managing VMware hosts.

In response to the second question, an extra set of credentials are required in order to manage the file system on ESX servers. Currently you have to use either the root account or take advantage or configure the VM Delegate functionality to use a different account. This is also mentioned in the comments below. If you don't need to manage file system operations, like copying files to or from ESX servers, then you don't need this additional set of credentials.

Finally, I've heard mention from several Microsoft employees that VMware does not have intelligent placement. That's not accurate. When DRS is enabled in a cluster of ESX hosts, VirtualCenter will both a) calculate the initial placement of the VM within the cluster and b) use VMotion to dynamically optimize the placement of the VM within the cluster while the VM is running.

[1]: {{< relref "2008-06-11-vir253-microsoft-system-center-vmm-2008-part-1-of-2.md" >}}
