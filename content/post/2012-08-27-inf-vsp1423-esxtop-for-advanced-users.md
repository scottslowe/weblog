---
author: slowe
categories: Liveblog
comments: true
date: 2012-08-27T15:58:34Z
slug: inf-vsp1423-esxtop-for-advanced-users
tags:
- ESXi
- Virtualization
- VMware
- VMworld2012
title: 'INF-VSP1423: esxtop for Advanced Users'
url: /2012/08/27/inf-vsp1423-esxtop-for-advanced-users/
wordpress_id: 2779
---

This is a session blog for INF-VSP1423, esxtop for Advanced Users, at VMworld 2012 in San Francisco, CA. The presenter is Krishna Raj Raja from VMware.

The presenter starts out the session with an explanation of how the session is going to proceed. This year---this session has been presented multiple years at multiple VMworld conferences---he's choosing to focus on exposing the inner workings of the ESXi hypervisor through esxtop. High-level topics include NUMA, CPU usage, host cache, SR-IOV, and managing data.

The session starts with managing data. The presenter suggests using vCenter Operations for managing data with large numbers of VMs, multiple vCenter instances, etc. However, in situations where you are focused on one host or one VM, then esxtop can be useful and pertinent. In many cases, though, vCenter Operations may be a better tool than esxtop.

He then provides a quick overview of managing the esxtop screen, such as proper TERM settings for Mac OS X (use `export TERM=xterm`), focusing on particular values, and showing/hiding counters. It's also possible to exclude data by exporting the esxtop entities (using `--export-entity file.lst`), editing that text file, then importing them back in using `--import-entity file.lst`.

Having now provided some details on managing the data, he switches his discussion to the topic of granted memory. He provides the example of assigning 8 GB of memory to a Windows 7 32-bit system, which is only capable of accessing 3 GB of RAM. Hence, Granted Memory will be limited to 3 GB. Switching to 64-bit shows that Granted Memory will change that to nearly 8 GB, as Windows touches all pages during boot. Later, that same value descreases, but the MCTLSZ counter increased to show that the memory balloon oontrol has reclaimed some of that memory.

Interestingly enough, setting a limit doesn't cause Granted Memory to be reduced to the limit. In this case, though, the Zero counter increased to show that multiple virtual memory pages were being pointed to a single memory page. Reducing the limit even further shows Granted Memory dropping and the MCTLSZ counter increased (meaning ballooning was active).

Next, the presenter talks about Active Memory. In many cases, when Active Memory is higher, CPU usage will also be higher. Similarly, when Active Memory is very low, CPU utilization is also low. This is not always the case, though. Be aware that Active Memory values can be reported as low if ballooning is active, even though CPU utilization might be high in that instance.

Next up is Overhead Memory. Found in the OVHDUW counter, this is influenced by a number of things; perhaps the biggest contributor is video memory. Setting large video memory settings can increase the overhead memory. In vSphere 5.0 and vSphere 5.1, VMware has been able to dramatically reduce overhead memory usage (in the example shown, it went from 260 MB of overhead in vSphere 4.x to 130 MB of overhead in vSphere 5.x). Less overhead memory means hosting more VMs. He also discussed OVHDMAX, but I couldn't catch the details other than to say that this value is also greatly reduced in vSphere 5.x.

With regard to NUMA, there is a counter called NHN (NUMA Home Node). When NHN has multiple values, that means RAM is being split (or interleaved) across multiple NUMA nodes. This occurs when the VM has been allocated more resources than can be satisfied using a single NUMA node. Even though the host is using multiple NUMA nodes, the guest OS still only sees it as a single NUMA node. vNUMA, introduced in hardware version 9 and applied to VMs with more than 8 vCPUs, means that NUMA information is exposed to the guest OS. This can be helpful in improving the performance of NUMA-aware applications like databases, Java application servers, etc. (Virtual hardware version 9 is introduced in vSphere 5.1.) _(CORRECTION: Although the presenter indicated that virtual hardware version 9 is required, documentation for vSphere 5.0 indicates that vNUMA only requires virtual hardware version 8.)_

Now the presenter moves on to user worlds. Any process that runs in the VMkernel is called a user world. In esxtop, the GID column indicates the group ID of a process (the ID for this world group), and the NWLD column indicates how many worlds are in that process. When %RUN is high for idle, that's perfectly normal and expected. There are world groups for VMs (and each VM has its own world group under the User world group). Pressing uppercase V, then esxtop will only display the VM world groups.

Let's look at an application of world groups. By expanding a world group, you can see the specific worlds within a world group, and that will give you better visibility into what might be causing higher CPU usage. In the example he shared, turning on 3-D support meant that a separate SVGA thread (or world) could cause very high CPU usage in esxtop, even though the guest was not showing significant CPU utilization. The MKS threads (mouse, keyboard, screen) can also drive up CPU utilization. An example would be high CPU usage for the MKS worlds, but not necessary high CPU usage for the SVGA 3-D thread.

Expanding the LUN display (by pressing e) can expose which world is contributing I/O to a particular LUN. From there, you can drill down into various screens within esxtop to determine exactly which thread/process/world is generating CPU usage as a result of the I/O being generated.

Now the presenter moves on to CPU usage, starting with physical CPUs. esxtop recognizes when hyperthreading is enabled, but vCenter Server doesn't distinguish logical CPUs and reports values twice per core. CPU core speeds are denoted by different P-states; P0 is the maximum rated clock speed, whereas P-n is the current clock speed. Different counters in esxtop might report different values because some are based on P-0, whereas others are based on P-n. Choosing "OS Control Mode" (or the equivalent) in the server BIOS then causes esxtop to report all the different P-states for the CPUs, with utilizations reported for each P-state. The PSTATE MHZ value shows the different clock speeds supported for the CPUs.

By default, the VMkernel tries to place (schedule) VMs on different cores, instead of just different logical CPUs (which might be hyperthreaded cores on the same physical core). This behavior can be modifed using the "Hyperthreaded Core Sharing" setting in vCenter Server; the default value for this setting is Any (which allows core sharing without any restrictions). For workloads that are sensitive to core sharing or caching behaviors, you can change this setting to optimize performance.

The presenter next went through a discussion of caching, but I wasn't able to catch all the details. He discussed both cache to SSD as well as context-based read caching (CBRC). CBRC explains why esxtop might report some level of I/O at the VM level, but very little I/O at the LUN level. (CBRC is also referred to as the View Storage Accelerator.)

SR-IOV---Single Root I/O Virtualization---isn't visible inside esxtop. SR-IOV allows a single physical device (or function; referred to as a PF) can present multiple virtual instances (or virtual functions, known as VFs) to the ESXi hypervisor. VFs can be presented directly to a guest OS using VMDirectPath. Running something like lspci in the guest OS shows the device being presented directly to the guest. esxtop will only show the physical NICs. There are other ways of viewing this information, though. You can look at CPU usage, or you can look at IRQs (interrupt requests), where esxtop will show a "pcip" device that denotes a PCI passthrough device (like an SR-IOV virtual function.)

At this point the presenter wrapped up the session.
