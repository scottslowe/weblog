---
author: slowe
categories: Liveblog
comments: true
date: 2008-09-18T12:49:38Z
slug: ta2644-networking-io-virtualization
tags:
- Networking
- Virtualization
- VMware
- VMworld2008
title: 'TA2644: Networking I/O Virtualization'
url: /2008/09/18/ta2644-networking-io-virtualization/
wordpress_id: 941
---

This is the liveblog for TA2644, Networking I/O Virtualization, presented by Pankaj Thakkar, Howie Xu, and Sean Varley.

The session starts out with yet another overview of VDC-OS. This session will focus on technologies that fall into the vNetwork infrastructure vService. The agenda for the session includes networking I/O virtualization, virtualized I/O, and VMDirectPath.

Starting out, the presenter first defines what exactly networking I/O virtualization is. Networking I/O virtualization is providing muxing/demuxing packets among VMs and the physical network. VMs need to be decoupled from physical adapters, and the networking I/O virtualization must be fast and efficient.

Now that the audience has an idea of what networking I/O virtualization is, the presenter changes focus to talk about the architecture that provides I/O virtualization. First, there is a virtual device driver that can either model a real device (e1000, vlance) or that can model a virtualization-friendly device (vmxnet, a paravirtualized device). I'm glad to see the vendor refer to this as a paravirtualized device, since that's really what it is.

Below the virtual device and virtual device driver, there is the virtual networking I/O stack. This is where features like software offloads, packet switching, NIC teaming, failover, load balancing, traffic shaping, etc., are found.

Finally, at the lowest layer, are the physical devices and their drivers.

The next discussion is tracing the life of a received packet through the virtualized I/O stack. After tracing the life of a packet, the presenter discusses some techniques to help reduce the overhead of network I/O. These techniques include zero copy TX, jumbo frames, TCP segmentation offload (large send offload and large received offload).

The problem with using jumbo frames, though, is that the entire network must be configured to support jumbo frames. Instead, the use of TSO (or LSO, as it is sometimes known) can help because it pushes the segmentation of data into standard size MTU segments down to the NIC hardware. This is fast, but even a software-only implementation of TSO can provide benefits.

(As a side note, it's difficult to really understand the presenter; he has a very thick accent.)

On the receive side, the technology called NetQueue is intended to help improve performance and reduce overhead. When the NIC receives the packet, it classifies the packet into the appropriate per-VM queue and notifies the hypervisor. The presence of multiple queues allows this solution to scale with the number of cores present in the hardware. It also looks like NetQueue can be used in load balancing/traffic shaping, although I'm unclear exactly how as I didn't understand what the presenter said.

Zero copy TX was discussed earlier (copy the packet from the VM directly to the NIC), but there was no discussion of zero copy RX. With NetQueue and VM MAC addresses being associated with the various queues, it's also possible to do zero copy RX. The caveat: the guest can access the data before it is actually delivered to it.

The focus on the presentation now shifts to a discussion of VMDirectPath, or I/O passthrough. This technology initiative from VMware requires hardware I/O MMU to perform DMA address translation. In this scenario, the guest controls the physical hardware and the guest will have a driver for that specific piece of hardware. VMDirectPath also needs a way to provide a generic device reset; FLR (Function-Level Reset) is a PCI standard that provides this.

SR-IOV (Single Root I/O Virtualization) is a PCI standard that allows multiple VMs to share a single physical device. If I understand correctly, this means that VMDirectPath will allow multiple VMs to share a single physical device via SR-IOV. Part of SR-IOV is creating virtual functions (VF) that the guest sees and mapping those to physical functions (PF) that the physical hardware controls and sees.

Challenges with VMDirectPath include:

* Transparent VMotion: Because the guest controls the device, there's no way to control device state, so VMotion won't be possible. This is logical and fully expected, but certainly has an impact on the usefulness of this technology.

* VM management: Users are now placed back into the issue of managing device drivers into VMs based on the hardware to which they are connected.

* Isolation and security: A lot of the features provided by the hypervisor (VMsafe, MAC spoofing, promiscuous mode, VMware FT, etc.) are lost when using VMDirectPath.

* No memory overcommitment: Physical devices will DMA into the guest memory, but this requires that memory overcommit is disabled so that this works.

Although these limitations around VMDirectPath are significant, there still can be valid use cases. Consider appliance VMs or special purpose VMs, such as a VM to share local storage or a firewall VM, where technologies like VMotion or VMware FT aren't necessary or aren't desired.

Generation 2 of VMDirectPath will attempt to address the challenges described above. One way of accomplishing that is called "uniform passthrough," in which there is a uniform hardware/software interface for the passthrough part. This allows a transparent switch between hardware and software from the hypervisor while the guest is not affected or even aware of the mode. This puts the control path under the control of the hypervisor, but bypasses the hypervisor for the data path.

This Gen2 implementation allows for migration because the mode is switched from direct to emulation transparently and without any special support within the guest OS.

Another way of implementing is described by Sean Varley of Intel. This method is called Network Plug-in Architecture. Most of the functionality in this solution is embedded inside the guest device driver, which typically would be VMware's paravirtualized vmxnet driver. Sean underscores the need for SR-IOV support in order to really take advantage of VMDirectPath, because it doesn't really scale otherwise.

This particular solution consists of a guest OS-specific shell and an IHV-specific hardware plug-in. The interface  of the guest OS-specific shell will be well-known and is the subject of a near-future joint disclosure between Intel and VMware. The plug-in will allow various other IHVs to write software that will allow their hardware to be used in this approach with VMDirectPath.

This plug-in also allows for emulated I/O, similar to what ESX offers today, in the event that SR-IOV support is not available or if the user does not want to use VMDirectPath. Upon initialization, the guest OS shell will load the appropriate plug-in and (where applicable) create a VF that maps onto a VMDirectPath-enabled physical NIC.

Migration in this scenario is enabled because the hypervisor remains in control of the state of the shell and the plug-in at all times. The hypervisor can reset the VF, migrate the VM, and then load a new plug-in on the destination via the initialization process described earlier.

The key advantages of this particular approach are IHV independence, driver containment, and hypervisor control. This enables IHV differentiation and removes the VM management headache described earlier (VMs won't need hardware-specific drivers). Hypervisor control is maintained because the SR-IOV split model of VF/PF is maintained, and the hypervisor controls plug-in initialization and configuration.

The session ends with a sneak preview demonstration of a migration using the plug-in architecture and VMDirectPath, migrating a VM between an SR-IOV-enabled NIC and a non-enabled NIC on a separate host. The presenter showed how the vmxnet driver loaded the appropriate plug-in based on the underlying hardware.
