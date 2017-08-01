---
author: slowe
categories: News
comments: true
date: 2008-07-08T19:09:55Z
slug: sanbolic-looking-to-capitalize-on-hyper-v-opportunity
tags:
- HyperV
- Microsoft
- SAN
- Storage
- Virtualization
title: Sanbolic Looking to Capitalize on Hyper-V Opportunity
url: /2008/07/08/sanbolic-looking-to-capitalize-on-hyper-v-opportunity/
wordpress_id: 757
---

Sanbolic, whose [Melio FS product I discussed][1] a short while ago, announced today the availability of their Kayo file system. The official press release is [here](http://www.sanbolic.com/pdfs/Sanbolic_Press_Release_Kayo_File_System_for_Hyper-V_Final%207-4.pdf) in PDF format. Quoting from the press release:

>Sanbolic today announced that Windows Server 2008 Hyper-V virtual machines can now be stored on a single shared storage area network (SAN) storage volume using Sanbolic Kayo File System. The virtual machines can then be moved independently between physical host servers using Quick Migration because all host servers have shared access to the virtual machines files. Kayo FS will be price at $299 per host server and sold in a 5 license bundle.

Kayo FS is described as "VMFS for Hyper-V," providing file level shared access to a shared SAN volume. This is distinguished from Sanbolic's advanced file system, Melio FS, which provides byte-range locking and can provide concurrent access to application data on a SAN. The use of either Kayo FS or Melio FS resolves a key problem with Hyper-V deployments that want to take advantage of Quick Migration functionality, and that is that each VM would require its own LUN.

The introduction of Kayo FS also removes the key objection to the use of Melio FS for Hyper-V deployments: price. Kayo FS will be priced much lower than Melio FS; this means organizations adopting Hyper-V will be much more likely to swallow the cost of Kayo FS vs. Melio FS.

[1]: {{< relref "2008-06-16-melio-fs-hyper-v-and-vmware-esx.md" >}}
