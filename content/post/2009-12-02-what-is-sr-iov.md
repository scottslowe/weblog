---
author: slowe
categories: Education
comments: true
date: 2009-12-02T23:39:37Z
slug: what-is-sr-iov
tags:
- Hardware
- HyperV
- Virtualization
- VMware
- Xen
title: What is SR-IOV?
url: /2009/12/02/what-is-sr-iov/
wordpress_id: 1760
---

I/O virtualization is a topic that has received a fair amount of attention recently, due in no small part to the attention given to [Xsigo Systems](http://www.xsigo.com) after their participation in the Gestalt IT [Tech Field Day](http://gestaltit.com/featured/top/stephen/tech-field-day-1/). While Xsigo uses InfiniBand as their I/O virtualization mechanism, there are other I/O virtualization technologies out there as well. One of these technologies is Single Root I/O Virtualization (SR-IOV).

So what is SR-IOV? The short answer is that SR-IOV is a specification that allows a PCIe device to appear to be multiple separate physical PCIe devices. The SR-IOV specification was created and is maintained by the PCI SIG, with the idea that a standard specification will help promote interoperability.

SR-IOV works by introducing the idea of _physical functions (PFs)_ and _virtual functions (VFs)_. Physical functions (PFs) are full-featured PCIe functions; virtual functions (VFs) are "lightweight" functions that lack configuration resources. (I'll explain why VFs lack these configuration resources shortly.)

SR-IOV requires support in the BIOS as well as in the operating system instance or hypervisor that is running on the hardware. Until very recently, I had been under the impression that SR-IOV was handled solely in hardware and did not require any software support; unfortunately, I was mistaken. Software support in the operating system instance or hypervisor is definitely required. To understand why, I must talk a bit more about PFs and VFs.

I mentioned earlier that PFs are full-featured PCIe functions; they are discovered, managed, and manipulated like any other PCIe device. PFs have full configuration resources, meaning that it's possible to configure or control the PCIe device via the PF, and (of course) the PF has full ability to move data in and out of the device. VFs are similar to PFs but lack configuration resources; basically, they only have the ability to move data in and out. VFs can't be configured, because that would change the underlying PF and thus all other VFs; configuration can only be done against the PF. Because VFs can't be treated like a full PCIe device, then the OS or hypervisor instance must be aware that they are not full PCIe devices. Hence, OS or hypervisor support is required for SR-IOV so that the OS instance or hypervisor can properly detect and initialize PFs and VFs correctly and appropriately. At this time, SR-IOV support is only found in some of the open source Linux kernels; this means it will find its way into KVM and Xen first. I do not have a timeframe for SR-IOV support in VMware vSphere or Microsoft Hyper-V.

So, putting this all together: what do you get when you have an SR-IOV-enabled PCIe device in a system with the appropriate BIOS and hardware support and you're running an OS instance or hypervisor with SR-IOV support? Basically, you get the ability for that PCIe device to present multiple instances of itself up to the OS instance or hypervisor. The number of virtual instances that can be presented depends upon the device.

The PCI SIG SR-IOV specification indicates that each _device_ can have up to 256 VFs. Depending on the SR-IOV device in question and how it is made, it might present itself in a variety of ways. Consider these exampes:

* A quad-port SR-IOV network interface card (NIC) presents itself as four devices, each with a single port. Each of these devices could have up to 256 VFs (single port NICs) for a theoretical total of 1,024 VFs. In this case, each VF would essentially represent a single NIC port.

* A dual-port SR-IOV host bus adapter (HBA) presents itself as one device with two ports. With 256 VFs, this would result in 512 HBA ports spread across 256 dual-port virtual HBAs.

These are, of course, theoretical maximums. Because each VF requires actual hardware resources, practical limits are much lower. Currently, 64 VFs seems to be the upper limit for most devices.

In situations where VFs represent additional NIC ports or HBA ports, other technologies must also come into play. For example, suppose that you had an SR-IOV-enabled Fibre Channel HBA in a system; that HBA could present itself as multiple, separate HBAs. Of course, because these logical HBAs would still share a single physical HBA port, you'd need NPIV (more information [here][1]) to support running multiple WWNs and N_Port_IDs on a single physical HBA port.

Similarly, you might have a Gigabit Ethernet NIC with SR-IOV support. That NIC could theoretically (according to the PCI SIG SR-IOV specification) present itself as up to 256 virtual NICs. Each of these NICs would be discrete and separate to the OS instance or hypervisor, but the physical Ethernet switch wouldn't be aware of the VFs. Switches wouldn't, by default, reflect some types of traffic arriving inbound on a port (from one VF) back out on the same port (to another VF). This could create some unexpected results.

SR-IOV does have its limitations. The VFs have to be the same type of device as the PF; you couldn't, for example, have VFs that presented themselves as one type of device when the PF presented itself as a different type of device. Also, recall from earlier that VFs generally can't be used to configure the actual physical device, although the extent to which this is true depends upon the implementation. The SR-IOV specification allows some leeway in the actual implementation; this leeway means that some SR-IOV-enabled NICs may also have VF switching functionality present (where the NIC could switch traffic between VFs without the assistance of a physical switch) while other NICs may not have VF switching functionality present (in which case VFs would not be able to communicate with each other without the presence of a physical switch).

I do want to point out that SR-IOV is _related_ to, but not the same as, hypervisor bypass (think VMDirectPath in VMware vSphere). SR-IOV enables hypervisor bypass by providing the ability for VMs to attach to a VF and share a single physical NIC. However, the use of SR-IOV does not automatically indicate the hypervisor bypass will also be involved. Hypervisor bypass is a topic that I'm sure I will discuss in more detail in the near future.

Finally, it's worth noting that the PCI SIG is also working on a separate IOV specification that allows multiple systems to share PCIe devices. This specification, known as Multi-Root IOV (MR-IOV), would enable multiple systems to share PCIe VFs. I hope to have more information on MR-IOV in the near future as well.

You now should have a basic understanding of SR-IOV, what it does, what is necessary to support it, and some of the benefits and drawbacks that SR-IOV creates. Feel free to post any questions you have about SR-IOV in the comments and I'll do my best to get answers for you.

[1]: {{< relref "2009-11-27-understanding-npiv-and-npv.md" >}}
