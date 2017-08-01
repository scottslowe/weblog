---
author: slowe
categories: Explanation
comments: true
date: 2010-10-28T23:15:13Z
slug: recoverpoint-and-vaai-interoperability
tags:
- EMC
- Interoperability
- Storage
- Virtualization
- VMware
- vSphere
title: RecoverPoint and VAAI Interoperability
url: /2010/10/28/recoverpoint-and-vaai-interoperability/
wordpress_id: 2145
---

As most of you probably already know, the addition of the vStorage APIs for Array Integration (VAAI) was a major new feature of VMware vSphere 4.1. For those of you not familiar with VAAI, the short story is that these APIs enable VMware ESX/ESXi to offload certain types of storage operations to VAAI-compliant storage arrays in order to increase efficiency.

This new functionality is supported on EMC's midrange Unified arrays with the release of FLARE 30 some time ago. VAAI support will come to the Symmetrix VMAX arrays later in the year with the next release of Enginuity. Similarly, EMC is extending VAAI support across the range of products, including EMC's heterogeneous storage replication product: EMC RecoverPoint.

While EMC is working to bring VAAI support to RecoverPoint---which is a larger task than it might seem, as I'll explain shortly---today that support isn't completely implemented. In this article, I'm going to describe in a bit more detail the interoperability between VAAI and RecoverPoint.

First, though, some basics. It's entirely possible that a fair number of readers might not be familiar with RecoverPoint, so I'll discuss some RecoverPoint basics first. RecoverPoint leverages the idea of _splitters_ to enable continuous data protection (CDP), continuous remote replication (CRR), and concurrent local and remote replication (CLR). CDP is like "the Tivo of storage", allowing users to create local replicas and rollback to virtually any point in time. CRR is similar, but with a remote array and enabling users to rollback to significant points in time. CLR, of course, combines the two. All of this is made possible by the splitters. RecoverPoint leverages either fabric-based splitters (such as Cisco SANTap), host-based splitters, or array-based splitters. These splitters take writes to protected LUNs and split them off to the appropriate local or remote replicas (or both).

I'm explaining all of this because RecoverPoint's support for VAAI varies based on the type of splitter that is in use. The following bullets provide more details on the interoperability between RecoverPoint and VAAI based on the splitter in use:

* If you are using an array-based splitter (EMC's Unified and CLARiiON arrays have an integrated RecoverPoint splitter), RecoverPoint **_will support_** hardware-assisted locking via the Compare and Swap (CAS) SCSI command. Hardware-assisted locking is also sometimes referred to as "Atomic Test and Set" (ATS), and it's the ability for VMware ESX/ESXi hosts to perform locking at a block level instead of a LUN level.

* All other VAAI commands (like hardware-assisted copy) **_are ignored_** (not rejected) by the CX splitter.

* With a fabric-based splitter, all VAAI commands **_are ignored_** (not rejected).

The fact that the VAAI commands are ignored instead of rejected is significant. When VMware ESX/ESXi attempts a VAAI command on a non-VAAI-capable array, the commands are typically rejected as not implemented. This tells VMware ESX/ESXi to "fall back" to pre-VAAI behavior---in other words, go back to the old way of doing things. In this case, the splitter is simply ignoring the command, not rejecting it. This means that the host doesn't know it should "fall back" and keeps issuing VAAI commands, and those commands aren't picked up by the splitter. The end result? Replicas that are corrupt and unusable, because some of the commands applied to the source were not captured and handled (but rather ignored) by the splitter. Clearly, this is not ideal behavior.

In this instance, the workaround is to actually disable the hardware-accelerated copy and hardware-accelerated zero VAAI commands at the VMware ESX/ESXi host level. These options are found in the Advanced Settings for a host:

* `DataMover.HardwareAcceleratedMove` controls the hardware-accelerated copy functionality; set the value to 0 to disable hardware-accelerated copy

* `DataMover.HardwareAcceleratedInit` controls the VAAI Write Same/Write Zero functionality; set the value to 0 to disable the feature

Because this value is host-wide, this has the unfortunate side effect of blocking VAAI functionality for all arrays to which the host is connected. (I'm currently looking for a way to disable VAAI functionality on a per-array or per-LUN basis, and I'll post more information here should I discover anything.) For now, though, this appears to be the only way to disable VAAI functionality, and you must disable this VAAI functionality or run the very likely risk of data corruption.

The good news is that future versions of RecoverPoint will improve in two ways. First, prior to full VAAI support, RecoverPoint will start rejecting the VAAI commands as not implemented. This will force VMware ESX/ESXi hosts to fall back to pre-VAAI behavior, and will allow the use of VAAI on other arrays that are not protected by RecoverPoint. This is actually more challenging than it appears because the EMC RecoverPoint developers must work closely with Cisco and Brocade so that their intelligent fabric splitters reject the commands as well. At some point in the future, EMC will work to bring full VAAI support to both array-based as well as fabric-based splitters. Again, this will require close development with Cisco and Brocade to bring VAAI support into the intelligent fabric splitters.

In summary:

1. The current version of RecoverPoint only supports hardware-assisted locking, and then only with the integrated array-based splitter in the Unified/CLARiiON arrays. The other VAAI commands are ignored.

2. Fabric-based splitters ignore all VAAI commands.

3. VAAI functionality that is ignored by RecoverPoint must be disabled on all applicable VMware ESX/ESXi hosts in order to avoid data corruption.

4. Future versions will properly reject VAAI commands as not implemented, instead of ignoring the commands.

As you can see, the current situation isn't the greatest in the world, but we are working to improve it. In the spirit of openness and transparency, I felt that readers need to know about this interoperability and how to properly use RecoverPoint in VMware vSphere 4.1 environments.

Please feel free to post any questions or clarifications in the comments below. Thanks!

**UPDATE:** RecoverPoint product management has clarified the behavior of the current release; see [my update post][1] for full details.

[1]: {{< relref "2010-11-01-recoverpoint-and-vaai-update.md" >}}
