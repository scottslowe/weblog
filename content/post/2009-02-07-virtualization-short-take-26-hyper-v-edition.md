---
author: slowe
categories: Information
comments: true
date: 2009-02-07T15:10:20Z
slug: virtualization-short-take-26-hyper-v-edition
tags:
- HyperV
- Microsoft
- Virtualization
- Windows
title: 'Virtualization Short Take #26, Hyper-V Edition'
url: /2009/02/07/virtualization-short-take-26-hyper-v-edition/
wordpress_id: 1170
---

I've been seeing an increasing amount of coverage on Hyper-V, especially with all the hype (sorry, no pun intended) surrounding Windows Server 2008 R2 and the next version of Hyper-V. Kudos to Ben Armstrong aka "Virtual PC Guy", who pumped out almost all of these articles and is really generating some good content. I don't want this to be a blatant rip-off of his content, but I did want to share this information with my readers. All the credit goes to Ben!

* Hyper-V and 3-D graphics in the parent partition apparently [don't mix](http://blogs.msdn.com/virtual_pc_guy/archive/2009/01/07/bad-performance-with-high-end-graphics-and-hyper-v.aspx). There isn't really a workaround. You can't really fault Microsoft for this---Hyper-V wasn't intended to be a high-end graphics workstation.

* Having problems with a corrupted virtual hard disk? [This information](http://blogs.msdn.com/virtual_pc_guy/archive/2009/01/07/how-do-i-fix-a-corrupted-virtual-hard-disk.aspx) may help.

* If you use Windows Sidebar, you might find the [Hyper-V Monitor Gadget](http://blogs.msdn.com/virtual_pc_guy/archive/2009/01/14/hyper-v-monitor-gadget-for-windows-sidebar-updated.aspx) handy. I don't use Windows Sidebar---I don't even use Windows unless I have to---so I can't provide any direct feedback on this tool.

* This blog post has information on how to [merge snapshots back into a single VHD](http://blog.networkfoo.org/?p=384).

* If you're using Hyper-V on a laptop, I would first ask you, "Why?" Then I would point you to this article by Ben on how to [enable/disable the Hyper-V hypervisor](http://blogs.msdn.com/virtual_pc_guy/archive/2009/01/21/hyper-v-r2-changes-for-no-hypervisor-booting.aspx) so as to allow the laptop to sleep or hibernate. These functions are disabled when the hypervisor is active.

* Running Windows 7 on Hyper-V works, but creates some [spurious event log entries](http://blogs.msdn.com/virtual_pc_guy/archive/2009/02/04/windows-7-on-windows-server-2008-hyper-v-and-false-event-logs.aspx) that can be ignored.

* In this post, Ben provides more information on [the upgrade path](http://blogs.msdn.com/virtual_pc_guy/archive/2009/01/22/upgrading-hyper-v-from-windows-server-2008-to-windows-server-2008-r2-beta.aspx) from Windows Server 2008 to Windows Server 2008 R2, including Hyper-V.

* Here's a guide on [how to migrate virtual machines](http://technet.microsoft.com/en-us/library/dd296684.aspx) from Virtual Server to Hyper-V (link [via Ben](http://blogs.msdn.com/virtual_pc_guy/archive/2009/02/03/virtual-server-to-hyper-v-migration-guide.aspx), again!).

* [Via Ben](http://blogs.msdn.com/virtual_pc_guy/archive/2009/01/27/how-to-handle-a-missing-but-still-running-virtual-machine.aspx), if you have a VM running Hyper-V but it's not showing up under the virtual machine list in the Hyper-V management console, restarting the virtual machine management service (VMMS) will likely correct the problem.

* For users looking forward to the live migration functionality included in Windows Server 2008 R2, Ben provides a link to this [step-by-step guide to setting up live migration](http://technet.microsoft.com/en-us/library/dd446679.aspx). There's in-depth information on configuring Cluster Shared Volumes (CSV) and setting up the cluster network for live migration.

That does it for now. Again, thanks to Ben Armstrong---and some of the other Microsoft bloggers who didn't make this list but whom I've referenced before---for putting out good information on their virtualization products. Keep up the good work!
