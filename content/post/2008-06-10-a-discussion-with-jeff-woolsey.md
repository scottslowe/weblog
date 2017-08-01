---
author: slowe
categories: Information
comments: true
date: 2008-06-10T21:53:12Z
slug: a-discussion-with-jeff-woolsey
tags:
- Citrix
- HyperV
- Microsoft
- TechEd2008
- Virtualization
- Windows
- Xen
title: A Discussion with Jeff Woolsey
url: /2008/06/10/a-discussion-with-jeff-woolsey/
wordpress_id: 730
---

I had the opportunity today to spend a few minutes in a one-on-one conversation with Jeff Woolsey, Senior Program Manager for Hyper-V at Microsoft. During our conversation, Jeff and I discussed the Hyper-V architecture, comparisons to other virtualization solutions, and some common competitive arguments for or against Hyper-V. I'd like to publish a summarized version of our conversation here. (Jeff, if you're reading and I've accidentally misquoted you, please be sure to correct me. I don't want to misrepresent any information. Keep me straight!)

I framed the discussion around a series of questions. Each question is listed below, along with a summary of the discussion resulting from that question.

**Question 1: What are the key architectural advantages of Hyper-V as compared to Xen or ESX?**

Jeff indicated that Hyper-V and Xen are architecturally very similar. Both use a privileged VM; Microsoft calls it the parent partition, Xen calls it dom0. In both cases, I/O is routed through this privileged partition and only the privileged partition has access to the physical hardware. Microsoft believes the hypervisor should be as thin as possible; Hyper-V is only about 600K worth of code. The networking stack and the storage stack are pushed up into the parent partition to keep drivers out of the hypervisor. Jeff referred me back to his session earlier in the day, where he discussed the need for the parent partition (my summary of that session is [here][1]). ESX puts all the drivers in the hypervisor, which means that they have a harder time providing support for new hardware (the example given was 4Gbps Fibre Channel HBAs vs. 8Gbps Fibre Channel HBAs). In talking about the placement of device drivers, our discussion naturally led us to the next question.

**Question 2: How would you respond to the concerns about the quality of the device drivers in the parent partition affecting the stability of the hypervisor?**

Jeff doesn't buy into this argument. Unlike desktops or workstations, administrators don't typically go willy-nilly with drivers on production servers. Drivers are generally provided by the hardware vendors. In addition, because Hyper-V requires the x64 edition of Windows Server 2008, this is even less of an issue; it's impossible to use unsigned drivers with x64 Windows. This means that any driver that can be used with Hyper-V will be WHQL-tested. Supposedly, this will keep out potentially faulty device drivers. Jeff pointed to the exclusive [use of Hyper-V to power the MSDN and TechNet web sites](http://blogs.technet.com/virtualization/archive/2008/05/20/msdn-and-technet-powered-by-hyper-v.aspx) at Microsoft as proof. I can see his point, but I still have to wonder if another level of qualification and validation shouldn't have been established to ensure that everything works as expected with Hyper-V. It still seems possible to me that organizations stepping outside the "Big 3" server vendors---Dell, HP, and IBM---could run into issues.

**Question 3: How much interoperability is there between Hyper-V and Citrix XenServer?**

Jeff admitted that this was outside his comfort area. I was specifically wondering about a Citrix claim of the ability to take a XenServer VM, move it to Hyper-V, and boot it right up. Jeff couldn't confirm if that was possible. He did indicate that there would be a hypercall adapter that would support paravirtualized Linux kernels designed to run under the Xen hypervisor as well as non-PV Linux kernels. As for interoperability of Integration Components (ICs) or paravirtualized drivers, Jeff wasn't sure. I should get more information on that soon and will post it here as soon as I receive it.

**Question 4: What is your response to complaints about Microsoft's support policy for third-party virtualization solutions?**

This discussion really got Jeff animated. He pointed me to an announcement that I apparently missed from this morning's keynote regarding the Server Virtualization Validation Program. This was mentioned in the [official press release](http://www.microsoft.com/presspass/press/2008/jun08/06-10TechEdITPro08PR.mspx), but I really don't recall seeing or hearing anything about it this morning. More information and more links about the SVVP, as I like to call it, is found in [this Windows Server Division Weblog post](http://blogs.technet.com/windowsserver/archive/2007/11/15/tech-support-for-all-those-different-vms.aspx). The idea behind the program is providing a framework to enable third-party virtualization solutions to qualify and validate running Windows Server 2008 on their hypervisor as well as providing a process for handling technical support cases, transferring cases between vendors, escalating cases, etc. Jeff tried to compare the idea of the Server Virtualization Validation Program to WHQL, which I can kind of see, but that analogy only goes so far. Without this program, I can certainly see Jeff's points regarding the complexity of who will be supported, what versions will be supported, how they will handle patches to the supported versions, etc. With this program, other virtualization vendors have a clearly defined process they can follow to get the same support as Hyper-V (or close to it).

In my mind, Microsoft must strike a very delicate balance here. Lots of people---competitors, partners, customers, resellers---see Microsoft's behavior here as anti-competitive, even if that isn't the case. Again, I can certainly see the complexities involved; it just seems like Microsoft perhaps should have moved more quickly to address this issue.

**Question 5: How would you address competitors' complaints about how Hyper-V requires a separate LUN for each VM for which Quick Migration functionality is needed?**

Jeff admitted that it is true that Hyper-V will require a separate LUN for each VM that needs Quick Migration functionality. Rather than spending time creating a clustered file system, Microsoft chose instead to allow storage partners to create those solutions. He referred me to [Sanbolic](http://www.sanbolic.com/), whose MelioFS clustered file system and LaScala volume manager will allow Hyper-V deployments to store multiple VMs on a single LUN visible to all hosts, just like VMFS. According to Jeff, this allows organizations to use both, if they so desire, instead of being "locked in." Using a third-party clustered file system eliminates the "one VM per LUN" limitation imposed when using Quick Migration.

In my opinion, relying on partners to fill certain portions of the solution is certainly a valid approach. Lots of companies do this. In this situation, though, it seems a bit odd. For other areas of their virtualization solution, Microsoft likes to tout the completeness of the solution. Consider the tight integration of the various System Center components with Windows Server 2008 and Hyper-V. But for this they want to rely on a partner? I have to wonder if a clustered file system developed by Microsoft isn't somewhere on the road map.

And that was the end of our discussion. I'd like to thank Jeff for taking the time to talk with me, answering my questions, and sharing his thoughts on these topics.

[1]: {{< relref "2008-06-10-vir367-hyper-v-security-and-best-practices.md" >}}
