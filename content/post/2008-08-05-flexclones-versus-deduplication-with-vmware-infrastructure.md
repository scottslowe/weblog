---
author: slowe
categories: Explanation
comments: true
date: 2008-08-05T11:42:26Z
slug: flexclones-versus-deduplication-with-vmware-infrastructure
tags:
- ESX
- NetApp
- ONTAP
- SAN
- Storage
- Virtualization
- VMware
title: FlexClones Versus Deduplication with VMware Infrastructure
url: /2008/08/05/flexclones-versus-deduplication-with-vmware-infrastructure/
wordpress_id: 786
---

A number of times over the last few months, I've run into situations where NetApp's FlexClone technology was being heavily pitched to customers interested in deploying, or expanding their deployment of, VMware Infrastructure.

In case you aren't familiar with the use of NetApp FlexClones in conjunction with VMware Infrastructure, have a look at these earlier articles of mine:

[How to Provision VMs Using NetApp FlexClones][1]  
[NetApp FlexClones with VMware, Part 1][2]  
[NetApp FlexClones with VMware, Part 2][3]  
[LUN Clones vs. FlexClones][4]

Now, after you've read all those articles (you _did_ read them, didn't you?), it should be fairly clear that using FlexClones can be very advantageous. However, those advantages come with some tradeoffs as well, most notably in the complete and total lack of integration with VMware Infrastructure itself.

This lack of integration means that users can't use VirtualCenter templates, because the cloning is taking place at the storage array instead of within VMware Infrastructure. This also means that customers can't apply customization specifications during the cloning process, so users will need to create their own Sysprep answer files and manually Sysprep the VMs before invoking the FlexClone process. Users are required to create scripts and tools to do simple things like using the VM name for the guest OS name during cloning.

&lt;aside&gt;Lest anyone think I'm picking on NetApp here, let me state that this would apply to any storage vendor that offers pointer-based copies. As long as the use of those pointer-based copies (or even deep copies, for that matter) is not integrated within VirtualCenter, then they will suffer the same problems.&lt;/aside&gt;

Deduplication, on the other hand, works seamlessly with VMware Infrastructure. This is primarily because the details of the deduplication are completely hidden; it all occurs "inside the box." Nothing needs to be configured within VirtualCenter; no VMs need to be modified. The NetApp storage system handles the details of the deduplication process itself, and VMware Infrastructure just consumes the storage.

Looking at these two technologies in that light, one might ask: why use FlexClones at all? If deduplication works seamlessly with VMware Infrastructure and FlexClones don't, then why bother? To be honest, there are some instances where FlexClones make sense---even with the lack of integration. Consider some of the examples listed below.

* In instances where a user needs to deploy _lots_ of VMs in a _very_ rapid fashion, FlexClones are much better. If time-to-deployment is the #1 driving factor, then FlexClones are the way to go. This could be particularly applicable and useful in VDI situations, as long as the broker doesn't mandate handling provisioning itself (like VDM does).

* In environments where provisioning and re-provisioning occurs on a frequent, regular basis, then FlexClones make sense. Even though large numbers of VMs aren't being provisioned, the time saved on frequent re-provisioning via FlexClones will not be insignificant.

* In situtations where there isn't sufficient storage for the VMs before they are deduplicated, FlexClones may be a better option. Deduplication is post-process, meaning that storage will be needed for the full datasets until deduplication runs. In situations where that isn't an option, then FlexClones can provide the same end benefit.

Personally, I'm of the opinion that unless an organization meets one of these criteria, then that organization should look to deduplication instead of FlexClones. Of course, that's just my personal opinion, and I'm open to hear what others have to say about the matter. NetApp gurus, feel free to weigh in.

[1]: {{< relref "2007-05-11-how-to-provision-vms-using-netapp-flexclones.md" >}}
[2]: {{< relref "2007-05-15-netapp-flexclones-with-vmware-part-1.md" >}}
[3]: {{< relref "2007-05-17-netapp-flexclones-with-vmware-part-2.md" >}}
[4]: {{< relref "2007-05-21-lun-clones-vs-flexclones.md" >}}
