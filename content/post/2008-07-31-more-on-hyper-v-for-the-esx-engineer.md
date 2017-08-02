---
author: slowe
categories: Explanation
comments: false
date: 2008-07-31T23:07:26Z
slug: more-on-hyper-v-for-the-esx-engineer
tags:
- HyperV
- Microsoft
- Virtualization
- VMotion
- VMware
- Windows
title: More on Hyper-V for the ESX Engineer
url: /2008/07/31/more-on-hyper-v-for-the-esx-engineer/
wordpress_id: 775
---

My colleague and friend, Aaron Delp, recently published a post titled [Hyper-V for the ESX Engineer](http://www.bladevault.info/2008/07/30/hyper-v-for-the-esx-engineer/). It's a good post, and provides a good overview of Hyper-V for someone who might already be familiar with VMware Infrastructure 3 (VI3). With sincere apologies to Aaron for plagiarizing his title, I thought I might add a few thoughts, comments, or clarifications to his post.

* Aaron mentions that Hyper-V is paravirtualized. Well, sort of. Hyper-V does support a paravirtualization interface (Hypercall, I believe?) for guest operating systems (i.e., Linux) that support it. In addition, future Windows guests will be "enlightened" as well. The confusing part about this is the fact that Microsoft (and Citrix, too) use the term "paravirtualization" to refer to the use of paravirtualized **drivers** instead of referring to the guest OS itself. Paravirtualized drivers are really nothing more than virtualization-optimized drivers, and it's possible to use paravirtualized drivers even when the guest OS has no idea it's being virtualized. In my mind, that's not the same as "true" paravirtualization. Note that VMware ESX supports VMI, another paravirtualization interface, for guests (i.e., Ubuntu Linux) that support it. Also keep in mind that **every** major virtualization vendor offers optimized/paravirtualized drivers, including VMware, Microsoft, Citrix, Virtual Iron, and Novell.

* You'll also see the base Windows Server 2008 (or Server Core) installation referred to as the "Parent Partition." All I/O travels through this installation. Whereas ESX uses a direct I/O model (drivers embedded in the virtualization engine), Hyper-V uses indirect I/O (drivers residing in the parent partition). Each side thinks their approach is the best, of course.

* Aaron makes some comparisons between Quick Migration and VMotion, which is understandable but not entirely appropriate. Quick Migration is not live migration, but is really more comparable to VMware HA. Quick Migration has some advantages over VMware HA (can be configured on a per-VM basis), but also has some disadvantages (requires a dedicated LUN for each VM for which Quick Migration is enabled). I've discussed [Quick Migration vs. Live Migration][1] before.

* Aaron briefly mentions SCVMM and expresses some doubt regarding using SCVMM 2008 (currently in beta, due to be released Q4) to manage VI3. It's certainly possible, and it does require VirtualCenter in order to work. Whether it's the home run that Microsoft hopes it will be is another story.

Thanks to Aaron for providing a good overview of Hyper-V. For more information, I'll refer readers to some of my Hyper-V and SCVMM session liveblogs from Tech-Ed back in June:

[VIR367: Hyper-V Security and Best Practices][2]  

[A Discussion with Jeff Woolsey][3]  

[VIR253: Microsoft System Center VMM 2008, Part 1 of 2][4]  

[VIR360: Microsoft System Center VMM 2008, Part 2 of 2][5]  

[VIR250: Advanced Storage Connectivity for VMs][6]  

[VIR358: Hyper-V Architecture, Scenarios, and Networking][7]  

[VIR350: System Center VMM Advanced Integration][8]

For even more resources, readers can also use [the HyperV tag](/tags/hyperv) to browse the site for all articles tagged for Hyper-V.

Oh, and if you don't have Aaron's RSS feed in your RSS reader, you're missing out. He's producing some really great stuff that you need to be reading. Go subscribe now!

[1]: {{< relref "2008-04-25-my-thoughts-on-the-live-migration-quick-migration-discussion.md" >}}
[2]: {{< relref "2008-06-10-vir367-hyper-v-security-and-best-practices.md" >}}
[3]: {{< relref "2008-06-10-a-discussion-with-jeff-woolsey.md" >}}
[4]: {{< relref "2008-06-11-vir253-microsoft-system-center-vmm-2008-part-1-of-2.md" >}}
[5]: {{< relref "2008-06-11-vir360-microsoft-system-center-vmm-2008-part-2-of-2.md" >}}
[6]: {{< relref "2008-06-11-vir250-advanced-storage-connectivity-for-vms.md" >}}
[7]: {{< relref "2008-06-12-vir358-hyper-v-architecture-scenarios-and-networking.md" >}}
[8]: {{< relref "2008-06-12-vir350-system-center-vmm-advanced-integration.md" >}}
