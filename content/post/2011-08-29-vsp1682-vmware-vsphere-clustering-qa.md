---
author: slowe
categories: Liveblog
comments: true
date: 2011-08-29T10:51:52Z
slug: vsp1682-vmware-vsphere-clustering-qa
tags:
- Virtualization
- VMware
- VMworld2011
- vSphere
title: 'VSP1682: VMware vSphere Clustering Q&A'
url: /2011/08/29/vsp1682-vmware-vsphere-clustering-qa/
wordpress_id: 2377
---

This is the session almost-live-blog (no wireless signal available in the session room) of VSP1682, VMware vSphere Clustering Q&A. The panelists are Duncan Epping, Frank Denneman, and Chris Colotti, all of VMware.

Lots of notable names were present in the session---Jason Boche, Kendrick Coleman, Mike Foley, Andy Banta, fellow co-authors Forbes Guthrie and Maish Saidel-Keesing were among the ones I noticed, and I'm sure that there were even more that I didn't notice. Clearly this session has received a lot of attention, due in no small part I'm sure to Duncan and Frank's successful vSphere 5 Clustering book.

As the session gets started and they open the floor for questions, the audience is a bit reluctant to get started with participation. The first question from the audience is in regard to a large-scale HA environment: what is the actual usable amount of bandwidth that you can/should allocate to an environment? Duncan answers with an observation that I would echo: very few environments are network constrained, even in 1Gbps network environments. Frank expands upon that discussion with a mention that higher network speeds for vMotion will affect DRS and how it will help DRS keep the cluster workload balanced.

The second question regards shared storage and what best practices might exist. Duncan answers first; one key consideration is the vSphere version; for example, what sort of servers could affect your architecture because of HA dependencies in pre-vSphere 5 environments. The question is really more of a storage design question than a clustering question, to be honest; while storage design and clustering design are linked, they are relatively independent. Frank adds that EVC (Enhanced vMotion Compatibility) and LUN access is another consideration, especially with very large (32 host) clusters. Identical configuration on all the hosts will help the DRS algorithms more effectively balance the cluster workload.

The third question concerns VM swap files and the interaction with Storage DRS. Frank answers that; he mentions that by default Storage DRS has a built-in VMDK affinity rule that keeps all the VM's virtual disks together. But what is the affect? Is there an effect on VM swap file performance or on Storage DRS? Frank mentions that he really doesn't think there will be an impact. The attendee follows up with a clarifying question about placement of the swap files; Frank indicates that placing them on slower (cheaper) disks might be a viable approach. The discussion then evolves into the use of auto-tiering arrays with Storage DRS; the general recommendation is to disable I/O metrics when using Storage DRS with auto-tiering arrays.

The next question is if there is a dependence on VAAI by Storage DRS. Duncan answers; there is no dependence on VAAI or VASA on Storage DRS. That being said, VAAI would be preferential to help offload Storage vMotion operations invoked by Storage DRS. The audience member has a unclear understanding of VAAI and VASA; he believes that I/O metrics and information are passed from the storage array to vSphere by VAAI/VASA. That is, of course, incorrect; Duncan points this out by explaining (again) the use of the I/O injector to gather I/O information from the array. Future versions might leverage VASA/VAAI to gather information.

The next question is about why someone should use HA in vSphere 5 if they are reluctant to implement HA due to problems with previous versions? Duncan responds that earlier versions of vSphere had a strong reliance on DNS; this reliance on DNS was removed/addressed in vSphere 5. I would agree with Duncan's response; name resolution was vitally important in HA environments prior to vSphere 5. Duncan mentions that the user will want to consider admission control policy and settings, but otherwise recommends that the user should enable HA. Duncan also goes into a more detailed description of storage heartbeating.

Next, a couple is asked about Storage DRS and VMs with virtual disks that live on different datastores. (The example given is database workloads with OS disks and data disks on separate tiers of storage.) What impact/benefit will this have with Storage DRS? Frank explains that Storage DRS uses both capacity (space available) and I/O metrics to determine recommendations. As a result, setting the appropriate latency on the datastore and datastore cluster will help drive SLAs, and suggests that using different tiers of disks might be unnecessary depending on the underlying disk structure. Or, as Duncan points out, you could use different Storage DRS datastore clusters.

Chris Colotti (who is acting as moderator) points out that Storage DRS is a new factor that will affect everyone's designs.

The next question is why someone should use HA when they are using a load balancer. Duncan's response is that, from an operational perspective, it helps address the failure of hardware nodes. Even with an F5 load balancer as this user has, someone still has to address the failure of the hardware node and restart VMs; HA can do that automatically even though hardware load balancers can help address traffic flows. Chris Colotti asks how many people are NOT using HA; a small number of people raise their hands.

Next, a user asks about the interaction of Microsoft clustering and vSphere HA/DRS. Duncan's response is that most people disable HA/DRS for Microsoft clusters, and that VMware is working on technologies that could replace Microsoft clustering. In the end, though, the answer for Microsoft clustering is still that you have to disable HA/DRS on the VMs participating in the cluster.

The next question is about the interaction of multiple HA clusters accessing a single datastore and the impact of that design decision. Duncan mentions that he masks datastores on a cluster level, instead of exposing a datastore to multiple clusters. This user also asks about VM failure monitoring; not many people are using it (based on the number of hands raised when Duncan asks how many are using it). It's not enabled by default, but it can help with guest OS failures. VMware has also opened the SDK for application monitoring. Frank adds that vCenter Server takes a guest OS console screenshot before resetting the VM, and Duncan answers a follow-up question about the interaction between VMware Tools and VM failure monitoring. The VM failure monitoring feature looks for network I/O, disk I/O, and the VMware Tools heartbeat to detect guest OS failure.

The next user asks about handling storage failures with vSphere HA; is there a way to handle storage array failure with vSphere HA? Duncan points out that HA will not address storage array failure. The customer mentions that EMC VPLEX will address the concern, but there are not any VMware software-based solutions to this need.

With regard to EVC, Frank highly recommends always enabling EVC on a cluster; there is no performance impact and it will help ensure processor compatibility between old and new hosts. In addition, enabling EVC on a cluster with VMs running could require some downtime in order for the CPU masks to get applied.

The next question is about VMFS-5 and the 2TB limit. What is the "sweet spot" with regards to datastore sizing? (I personally would say the answer depends on many different factors, not the least of which is the underlying storage architecture.) Frank asks the question back about how many VMs he's comfortable placing on a single datastore. Many of the attendees are still using only a few VMs per LUN; I'm curious to know why this is the case. Is it I/O driven? Is it based on time to backup and time to recovery? That's not clear to me, and if any readers would like to provide some feedback in the comments that would be great.

The next user asks about array-based snapshots and Storage DRS; how do the two interact? Replication, snapshots, deduplication, and thin provisioned LUNs are all affected by Storage DRS. You will definitely want to adjust operational procedures and how you use array-based features like snapshots and replication with Storage DRS. Frank recommends using Storage DRS for initial placement, but don't use I/O metrics and don't enable migrations.

The next user asks if Storage DRS and SRM work together; Duncan mentions that it breaks replication but doesn't break SRM. I thought I recalled that you couldn't use datastore clusters at all with SRM, but I don't remember where I saw that. Anyone have more information on that?

There were a few more questions, but I had to shut down and prepare for my next session, which is my vSphere design session (VSP1926).
