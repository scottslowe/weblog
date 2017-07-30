---
author: slowe
categories: Liveblog
comments: true
date: 2012-08-28T16:40:00Z
slug: inf-bco2655-ft-for-smp-vms-tech-preview
tags:
- Virtualization
- VMware
- VMworld2012
title: 'INF-BCO2655: FT for SMP VMs Tech Preview'
url: /2012/08/28/inf-bco2655-ft-for-smp-vms-tech-preview/
wordpress_id: 2799
---

This is a session blog for INF-BCO2655, titled "VMware vSphere Fault Tolerance for Multiprocessor Virtual Machines - Technical Preview." The presenters are both from VMware (naturally); they are Jim Chow and Shrinand Javadekar. I'm looking forward to seeing what progress has been made on this front since the last time VMware spoke about the possibility of multi-vCPU FT.

The session starts with a quick review of the various vSphere protection mechanisms: NIC teaming, storage multipathing (component failure); HA, FT (host failure), SRM (site failure). This quickly leads into a discussion on FT specifically as a means of providing continuous availability. Continuous availability is marked by zero downtime, zero data loss, no loss of TCP connections, and complete transparency to guest software (no dependency on guest OS/software, and no application learning/configuration).

The presenters next provide a quick overview of how the current version of FT works with single processor VMs. This involves the use of the vLockstep protocol, the FT logging network, and shared VMDK storage.

The milestone that enables multiprocessor FT is the SMP FT protocol (it replaces vLockstep). In addition, the FT logging network needs to go from 1 gigabit to 10 gigabit, and instead of using shared VMDK storage multi-processor FT will use mirrored/synchronized VMDK storage.

When enabling multiprocessor FT, you are essentially creating two VMs: the primary VM and the secondary VM. The second VM is an exact clone of the first VM, but it has its own set of complete VM files (including configuration files, VMDKs, etc.). Although SMP FT uses separate VMDK files (which could potentially be on separate datastores), the hosts on which the primary and secondary VMs must share a common datastore known as the tie break datastore. Essentially, the tie break datastore serves as a backup to the FT logging network connection.

SMP FT requires that the hosts running the primary and secondary VMs are vMotion compatible; this means that you can't pair Intel and AMD, for example.

After reviewing the process for enabling SMP FT, the presenters move into a live demonstration of SMP FT in an "anonymous" data center (in Palo Alto). The demonstration shows how to protect an SMP vCenter Server to make vSphere's central management component highly resistant to downtime. The presenters also demonstrated the ability to use non-disruptive VADP snapshots on SMP FT-protected VMs, which is a big improvement over the current implementation of FT (which doesn't support snapshots).

That concludes the session.
