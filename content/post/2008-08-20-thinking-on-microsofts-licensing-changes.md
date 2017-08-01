---
author: slowe
categories: Musing
comments: true
date: 2008-08-20T11:54:20Z
slug: thinking-on-microsofts-licensing-changes
tags:
- ESX
- ESXi
- HyperV
- Microsoft
- Virtualization
- VMotion
- VMware
- VMwareHA
- Windows
title: Thinking on Microsoft's Licensing Changes
url: /2008/08/20/thinking-on-microsofts-licensing-changes/
wordpress_id: 833
---

Much has transpired since yesterday, when I [urged VMware to join the SVVP][1] and get their software validated for full support by Microsoft. Since that time, it has come to light that VMware has joined the SVVP, although a formal announcement has not yet been made, and Microsoft has announced some significant licensing changes regarding virtualization. I've been reading the various announcements and analyses regarding this information and I thought it might be beneficial to try to pull all this together.

First, refer to [Patrick O'Rourke's blog entry](http://blogs.technet.com/virtualization/archive/2008/08/19/Thoughts-on-today_2700_s-virtualization-licensing-and-support-news.aspx), which does a great job of summarizing the need for application mobility licensing. Clearly, customers needed the ability to move applications freely between physical servers, and Microsoft themselves needed to allow customers to do this now that they have a more robust virtualization solution in place (Hyper-V and SCVMM 2008). While the licensing changes do benefit all virtualization vendors, it's important to note that Microsoft needed these changes for themselves as well.

Patrick's post also brings to light that while VMware has joined the SVVP, cooperative support is not yet in place. That won't come until validation via SVVP is completed, which may take some time. The joining of SVVP was necessary, as it is merely one step toward a larger goal.

However, there's more here than perhaps many people are realizing. Fortunately, there are a number of sites out there pointing out important caveats to the new licensing changes:

* Rich at VM /ETC correctly points out that the new licensing [**does not** apply to the Windows Server OS](http://vmetc.com/2008/08/19/new-microsoft-application-mobility-brief-does-not-cover-the-windows-operating-system/) itself. So you are still going to have problems with VMware HA and VMware DRS automatically moving VMs from server to server unless you use Windows Server Datacenter Edition (see below).

* Chris Wolf points out (both on his [personal blog](http://www.chriswolf.com/?p=184) as well as the [Data Center Strategies blog](http://dcsblog.burtongroup.com/data_center_strategies/2008/08/interpreting-mi.html)) that the lift on the 90-day license transfer does not apply to licenses purchased outside of a volume license agreement. Using OEM licenses? Then you're out of luck; those licenses still fall under the old restrictions.

* eWeek's Joe Wilcox points out that because the Windows Server OS isn't included in the 90-day license relief, some customers will simply license Windows Server Datacenter Edition for every CPU in their data center. Of course, the fact that you now get Hyper-V for free with Windows puts Microsoft...ahem, ahead of the game, shall we say? Read Joe's full report [here](http://www.microsoft-watch.com/content/server/microsofts_90_day_sleight_of_hand.html?kc=MWRSS02129TX1K0000535).

So, while Microsoft's licensing changes are a good first step, there's still more work to be done. Let's applaud the changes, which were necessary, but let's continue to press Microsoft to fix the issues that remain.

[1]: {{< relref "2008-08-19-a-quick-note-to-vmware.md" >}}
