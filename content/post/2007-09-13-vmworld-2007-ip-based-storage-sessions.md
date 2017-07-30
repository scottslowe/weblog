---
author: slowe
categories: Liveblog
comments: true
date: 2007-09-13T17:58:07Z
slug: vmworld-2007-ip-based-storage-sessions
tags:
- ESX
- iSCSI
- NAS
- NFS
- Storage
- Virtualization
- VMware
- VMworld2007
title: VMworld 2007 IP-Based Storage Sessions
url: /2007/09/13/vmworld-2007-ip-based-storage-sessions/
wordpress_id: 542
---

I just wrapped up two different sessions on IP-based storage, one on iSCSI configuration and one on performance characteristics and comparisons between iSCSI and NFS.

I couldn't liveblog the first session because it was too crowded (no room to type on my laptop) and I couldn't get a wireless signal from the VMworld 2007 network. There are, however, a couple key points from the session that stick out in my mind:

* ESX Server does not currently support MCS (multiple connections per session) or jumbo frames, two key optimizations that can really help with iSCSI performance. There is no word yet on when those shortcomings will be addressed; personally, I'm hoping that VMware fixes them in ESX 3.5.

* There is, apparently, some way of performing manual load balancing of iSCSI LUNs to help improve performance. The speakers did not go into any great details, and I was unable to speak with one of the presenters, Jon Hall, after the session. He did, however, invite me to contact him via e-mail, so I'll post more information on that once I've had some communication with him.

Most of the rest of the information presented in that session was pretty straightforward and was information I'd already seen. All in all, it was a decent session, but I didn't as much information from the session as I had hoped I would.

After lunch, I returned to the Moscone Center for a session titled "NFS and iSCSI - Performance Characterization and Best Practices." I was really hoping to get some additional best practices on using NFS and iSCSI and on maximizing performance with these IP-based storage solutions.

The session started with some performance characteristics with ESX Server today vs. ESX Server 3.0.1; basically, it was an update of some performance data presented last year at VMworld 2006.

These updated performance statistics are intended to show the results of optimizations that have been incorporated into ESX Server. This includes optimizations like improved and more accurate CPU accounting (this improves load balancing across VMs), improved PAE support, minimized NUMA overhead, improved CPU cost per I/O, increased maximum transfer sizes, and the ability in handle more concurrent I/Os.

As a result, software iSCSI sees a range of improvements since ESX Server 3.0.1, as high as 15% for 8K block writes, with reductions in latency across the board and reducions in CPU utilization as well. Read operations will show the greatest improvements.

Hardware iSCSI sees dramatic improvements in smaller block sizes, but the larger block sizes are essentially unchanged. The same goes for latency, and the reductions in CPU utilization for hardware iSCSI shares the same characteristics as for software iSCSI (but keep in mind that the absolute change---as opposed to percentage change---will be greater for software iSCSI). With hardware iSCSI, mixed read-write operations will benefit more than just read options.

Differences between VMFS and RDM (raw device mapping) are inconsequential (less than 2.5%); the only significant difference is CPU utilization, where VMFS requires more CPU time than RDM.

Comparisons of hardware iSCSI, software iSCSI, and NFS with regards to throughput show figures that are not entirely unexpected. NFS is slightly slower than both flavors of iSCSI, and has greater latency than the iSCSI flavors. However, all of the measured figures were in milliseconds, so it's not terribly significant.

Moving into performance best practices, the presenter started with the storage array itself, and provided the typical list of items to consider: total spindle count, number of spindles allocated for use, RAID level and stripe size, storage processor specifications, read/write cache sizes, and caching policies. This is all pretty standard information that is applicable in sizing a correct storage solution, independent of a virtualization implementation. (I would use those same counters to size a Microsoft Exchange storage solution or an Oracle storage solution, for example.)

Since we are talking IP-based storage, networking configuration comes to play here, including such things as the network topology, switches, NICs, flavor of iSCSI (hardware/software). Similarly, things about the ESX Server host like CPU speed and number of CPU cores, overall system architecture, bus speed, I/O subsystems, and memory configuration all play a part in determining performance of IP-based storage solutions.

Finally, it's important to understand the characteristics of the workload(s), such as I/O sizes, read/write patterns, and dependence upon aggregate throughput or latency.

&lt;aside&gt;OK, can we move past this stuff now? This is all basic stuff that isn't necessarily specific in any way to virtualization. I want to see best practices for using IP-based storage with VMware!&lt;/aside&gt;

To increase the overall throughput, using multiple NFS mount points may improve aggregate throughput as the cost of slightly higher CPU cost. NFS export options can affect performance as well. (OK, which options? Telling us that without telling us which options is kind of like leaving us hanging.)

iSCSI digests may or may not have an impact on performance; iSCSI header digests have little or no impact; turning off iSCSI data digests can improve performance.

The presenter went over some additional troubleshooting tips, and the slide briefly mentioned the `vsish` command. I hadn't heard of that command; anyone know of where I can find some additional information on `vsish`?

Wrapping up the session, the presenter went through a few scenarios involving performance troubleshooting with both iSCSI and NFS. Overall, I did not find this session to be nearly as helpful as I had hoped it would be, the presenter was not engaging, and the presentation did not provide the kind of detailed information that I felt should have been included. (Examples: mentioning that some NFS export options affect performance, but failing to mention which options, or stating that there is a VMware knowledge base article about a topic but failing to provide the KB article number or URL).
