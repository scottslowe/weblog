---
author: slowe
categories: Explanation
comments: true
date: 2008-06-16T14:55:10Z
slug: melio-fs-hyper-v-and-vmware-esx
tags:
- ESX
- HyperV
- Storage
- TechEd2008
- Virtualization
- VMware
title: Melio FS, Hyper-V, and VMware ESX
url: /2008/06/16/melio-fs-hyper-v-and-vmware-esx/
wordpress_id: 742
---

Last week Network World published [an article](http://www.networkworld.com/news/2008/061108-sanbolic-vmware-virtual-machines.html?page=1) about Sanbolic's announcement to provide VMware virtual machines access to shared application data using their shared file system, Melio FS. This announcement was made last week at Tech-Ed 2008 in Orlando, FL. Sanbolic also announced support for running Melio FS in VMware Infrastructure 3 (VI3) environments as well. Unfortunately, the information in these announcements seems to have gotten jumbled while being reported and now these two very different uses of Sanbolic's products are being intermingled when they really shouldn't be. Allow me to explain.

Readers who followed my Tech-Ed coverage will recall that I had the opportunity to speak one-on-one with Jeff Woolsey, Senior Program Manager for Virtualization at Microsoft. One of the questions I asked him during that session was about Hyper-V's requirement of one VM per LUN when Quick Migration was involved. Quoting from [my summary of that discussion][1]:

>Rather than spending time creating a clustered file system, Microsoft chose instead to allow storage partners to create those solutions. He referred me to Sanbolic, whose MelioFS clustered file system and LaScala volume manager will allow Hyper-V deployments to store multiple VMs on a single LUN visible to all hosts, just like VMFS.

Prompted by Jeff's response, I went to the Sanbolic booth at Tech-Ed and spoke with Bill Stevenson, Sanbolic's executive chairman and the same person quoted in the Network World article. Bill and I spoke at length and he shared a lot of information with me about Melio FS, LaScala, Hyper-V, and VMware ESX.

Here's what [the Network World article](http://www.networkworld.com/news/2008/061108-sanbolic-vmware-virtual-machines.html?page=2) says about Sanbolic and Hyper-V:

>Sanbolic for now isn't providing shared access to application data on Hyper-V, because Microsoft's upcoming hypervisor lacks the ability to store virtual machine images in shared SAN volumes, Stevenson says.

The first part of that statement is true. Sanbolic _hasn't_ announced support for allowing Hyper-V **VMs** to use Melio FS to access shared application data. However, Sanbolic _has_ announced support for running Melio FS (and the La Scala volume manager) on Windows Server 2008 Hyper-V **hosts**. This allows the VHDs for multiple VMs to be co-located with each other on the same LUN but still be able to take advantage of Quick Migration. It eliminates the "one VM per LUN" limitation, essentially.

With regards to the second part of that statement, I believe that the reason Sanbolic hasn't made any announcements regarding Melio FS inside Hyper-V VMs is that we don't yet know how Hyper-V is going to handle physical disk access. We know that there is support within Hyper-V for NPIV (refer to [this Tech-Ed session liveblog][2]), but I saw only a single mention of physical disk access---what I believe to be the equivalent of VMware's Raw Disk Mapping. Will Hyper-V support the equivalent of RDMs? I don't know, and Sanbolic probably doesn't know, either. Until they do know, they can't run Melio FS inside a Hyper-V VM.

And that's the crux of the VMware-related announcement: it's not about running Melio FS on the ESX **hosts**, but instead about running Melio FS inside Windows-based **VMs** on ESX. Sanbolic requires direct disk access and VMware provides that ability via RDMs, so Sanbolic can allow organizations running VMware to utilize VMFS as the shared file system for the hosts and Melio FS as the shared file system for the guests. By the way, I'm waiting to hear back from Sanbolic as to whether physical mode RDMs or virtual mode RDMs are supported. I'll post an update here when I find out.

The confusing part about the Network World article is that it doesn't, in my opinion, clearly delineate these two very different use cases. The idea of running Melio FS on the host vs. running Melio FS in the guest are two very different use cases with very different results, and the article seems to intermingle the two more than it should. This statement especially confused me:

>Gartner's Paquet says VMware could choose to provide the shared access to application data within the virtualization engine, making the Sanbolic technology unnecessary. VMware officials did not respond last week whether they will take such a step.

What, VMware writing a share file system for Windows so that Windows VMs could access SAN LUNs concurrently? What sense does that make? Believe me, there are some real geniuses over at VMware, and I would imagine that there is some magic that can be played at the virtualization layer that would make guest-level shared file systems unnecessary. But would that "magic" be efficient? Would it be worthwhile to develop when companies like Sanbolic already have this offering? Not in my mind. The purpose of VMFS is not to provide shared file system functionality for VMs; it's to provide that functionality for hosts.

To summarize:

* In a VI3 environment, you use Melio FS (and La Scala, if you desire) inside a VM with an RDM to allow multiple VMs to access a SAN LUN concurrently. This is something that NTFS can't/doesn't do, so Melio FS provides that functionality.

* In a Hyper-V environment, you run Melio FS (and La Scala, if you desire) on the Hyper-V hosts. This allows multiple hosts to access a SAN LUN concurrently, eliminating the "one VM per LUN" limitation imposed by Quick Migration.

Anyway, this can be a fairly complicated topic to understand and I wanted to try to help explain it as I understand it. I hope this has been helpful. If I'm wrong, feel free to correct me in the comments. Thanks!

**UPDATE:** Sanbolic updated me to inform me that they have thus far only tested physical mode RDMs with Melio FS. They are currently in the process of testing virtual mode RDMs.

[1]: {{< relref "2008-06-10-a-discussion-with-jeff-woolsey.md" >}}
[2]: {{< relref "2008-06-11-vir250-advanced-storage-connectivity-for-vms.md" >}}
