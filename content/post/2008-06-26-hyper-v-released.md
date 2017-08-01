---
author: slowe
categories: News
comments: true
date: 2008-06-26T10:55:37Z
slug: hyper-v-released
tags:
- HyperV
- Microsoft
- Virtualization
title: Hyper-V Released
url: /2008/06/26/hyper-v-released/
wordpress_id: 749
---

Today Microsoft released Hyper-V 1.0, their bare-metal hypervisor. I haven't been able to find much information on the Internet about it yet other than [Alessandro's announcement](http://www.virtualization.info/2008/06/release-microsoft-hyper-v-10.html) and [this article at Network World](http://www.networkworld.com/news/2008/062608-microsoft-ships-hyperv.html?nlhtmn=ts_062608&nladname=062608microsoftal); there was nothing on the Microsoft web site about it at the time of this writing.

I've covered Hyper-V extensively in the last few weeks due to my Tech-Ed coverage, so I'll refer readers back to some of the Hyper-V related sessions for more information on Hyper-V architecture, networking, security, or storage:

[VIR367: Hyper-V Security and Best Practices][1]  

[VIR250: Advanced Storage Connectivity for VMs][2]  

[VIR358: Hyper-V Architecture, Scenarios, and Networking][3]  

[Significant Networking Problem with Hyper-V][4]  

[Hyper-V Clustering Scenarios][5]  

[More on Hyper-V and NIC Teaming][6]

The release of Hyper-V now opens the door for Microsoft's System Center team to finalize Virtual Machine Manager 2008, which I currently see as a notable competitive advantage over VMware due to VMM's ability to manage both Hyper-V as well as VI3. There are a number of VMM 2008 posts available with more information also:

[VIR253: Microsoft System Center VMM 2008, Part 1 of 2][7]  

[VIR360: Microsoft System Center VMM 2008, Part 2 of 2][8]  

[VIR350: System Center VMM Advanced Integration][9]

The next few weeks will be interesting to watch; in particular, I'm interested to see what actions VMware will take to counter this new competitive threat.

**UPDATE:** Microsoft finally provided some URLs to their announcements:

[Microsofts Hypervisor Technology Gives Customers Combined Benefits of Windows Server 2008 and Virtualization](http://www.microsoft.com/presspass/features/2008/jun08/06-26hyperv.mspx)  

[Its here! Windows Server 2008 Hyper-V is available for download](http://blogs.technet.com/stbnewsbytes/archive/2008/06/26/it-s-here-windows-server-2008-hyper-v-is-available-for-download.aspx)

[1]: {{< relref "2008-06-10-vir367-hyper-v-security-and-best-practices.md" >}}
[2]: {{< relref "2008-06-11-vir250-advanced-storage-connectivity-for-vms.md" >}}
[3]: {{< relref "2008-06-12-vir358-hyper-v-architecture-scenarios-and-networking.md" >}}
[4]: {{< relref "2008-06-12-significant-networking-problem-with-hyper-v.md" >}}
[5]: {{< relref "2008-06-22-hyper-v-clustering-scenarios.md" >}}
[6]: {{< relref "2008-06-23-more-on-hyper-v-and-nic-teaming.md" >}}
[7]: {{< relref "2008-06-11-vir253-microsoft-system-center-vmm-2008-part-1-of-2.md" >}}
[8]: {{< relref "2008-06-11-vir360-microsoft-system-center-vmm-2008-part-2-of-2.md" >}}
[9]: {{< relref "2008-06-12-vir350-system-center-vmm-advanced-integration.md" >}}
