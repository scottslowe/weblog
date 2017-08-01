---
author: slowe
categories: Information
comments: true
date: 2009-11-18T09:00:00Z
slug: virtualization-short-take-31
tags:
- EMC
- ESXi
- HyperV
- Microsoft
- Virtualization
- VMware
- VMwareSRM
- vSphere
title: 'Virtualization Short Take #31'
url: /2009/11/18/virtualization-short-take-31/
wordpress_id: 1744
---

Welcome back to yet another Virtualization Short Take! Here is a collection of virtualization-related items---some recent, some not, but hopefully all interesting and/or useful.

* Matt Hensley [posted](http://matthensley.wordpress.com/2009/10/11/srm-4-0-and-emc-celerra-how-to-guide/) a link to [this VIOPS document](http://viops.vmware.com/home/servlet/JiveServlet/download/1602-1-8390/EMC%20Celerra%20NFS%20CLI%20%26%20SRM%204.0%20Setup%20Guide.pdf) on how to setup VMware SRM 4.0 with an EMC Celerra storage array. I haven't had the chance to read through it yet.

* Jason Boche informs us that both Lab Manager 3 and Lab Manager 4 have problems with the VMXNET3 virtual NIC. In [this blog post](http://www.boche.net/blog/index.php/2009/11/07/lab-manager-4-installation-fails-with-vsphere-vmxnet-3-nic/), Jason describes how his attempts to install Lab Manager server into a VM with the VMXNET3 NIC was failing. Fortunately, Jason provides a workaround as well, but you'll have to read his article to get that information.

* Bruce Hoard over at Virtualization Review (disclaimer: I write a regular column for the print edition of _Virtualization Review_) stirred up a bit of controversy with his post about [Hyper-V's three problems](http://virtualizationreview.com/blogs/the-hoard-facts/2009/11/hyper-v-problem.aspx). The first problem is indeed a problem, but not an architectural or technological problem; VMware is indeed the market leader and has a quite solid user base. The second two "problems" stem from Microsoft's architectural decision to embed the hypervisor into Windows Server. Like any other technology decision, this decisions has its advantages and disadvantages (these technology decisions are a real [double-edged sword][1]). Based on historical data, it would seem that the need to patch Windows Server will impact the uptime of the Windows virtualization solution; however, this is not to say that VMware ESX/ESXi are not without their patches and associated downtime as well. I guess the key takeaway here is that VMware seems to be doing a much better job of lessening (or even removing) the impact of the downtime through things like VMotion, DRS, HA, maintenance mode, and the like.

* Apparently there is a problem with the GA release of the Host Update utility that is installed along with the vSphere Client, as [outlined here](http://virtualisedreality.wordpress.com/2009/11/04/vsphere-upgrade-the-partition-needs-to-be-at-least-3040-mb-error/) by Barry Coombs. Downloading the latest version and reinstalling seems to fix the issue.

* And while we are on the subject of ESX upgrades, here's [another one](http://www.virtuallifestyle.nl/2009/10/upgrade-esx-to-v4-fails-boot-too-small/): if the /boot partition is too small, the upgrade to ESX 4.0.0 will fail. This isn't really anything too new and, as Joep points out, is documented in the [_vSphere Upgrade Guide_](http://www.vmware.com/pdf/vsphere4/r40/vsp_40_upgrade_guide.pdf). I prefer clean installations of VMware ESX/ESXi anyway.

* Dave Mishchenko details his adventures ([part 1](http://www.vm-help.com/esx40i/manage_without_VI_client_1.php), [part 2](http://www.vm-help.com//esx40i/manage_without_VI_client_2.php), and [part 3](http://www.vm-help.com//esx40i/manage_without_VI_client_3.php)) in managing ESXi without the VI Client or the vCLI. While it's interesting and contains some useful information, I'm not so sure that the exercise is useful in any way other than academically. First of all, Dave enables SSH access to ESXi, which is unsupported. Second, while he shows that it's _possible_ to manage ESXi without the VI Client or the vCLI, it don't seem to be very efficient. Still, there is some useful information to be gleaned for those who want to know more about ESXi and its inner workings.

* I think [Simon Seagrave](http://www.techhead.co.uk) and [Jason Boche](http://www.boche.net/blog) were collaborating in secret, since they both wrote posts about using vSphere's power savings/frequency scaling functionality. [Simon's post](http://www.techhead.co.uk/saving-power-with-vmware-vsphere-esx-dynamic-voltage-and-frequency-scaling-dvfs) is dated October 27; [Jason's post](http://www.boche.net/blog/index.php/2009/11/11/tame-electrical-and-heating-costs-with-cpu-power-management/) is dated November 11. Coincidence? I don't think so. C'mon, guys, go ahead and admit it.

* Thinking of using the Shared Recovery Site feature in VMware SRM 4.0? This [VMware KB article](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1014640) might come in handy.

* I'm of the opinion that every blogger has a few "masterpiece" posts. These are posts that are just so good, so relevant, so useful, that they almost transcend the other content on the blogger's site. Based on traffic patterns, one of my "masterpiece" posts is the one on [ESX Server, NIC teaming, and VLAN trunking][2]. It's not the most well-written post I've ever published, but it seems to have a lasting impact. Why do I mention this? Because I believe that Chad Sakac's post on [VMware I/O queues, microbursting, and multipathing](http://virtualgeek.typepad.com/virtual_geek/2009/06/vmware-io-queues-micro-bursting-and-multipathing.html) is one of his "masterpiece" posts. [Like Scott Drummonds](http://vpivot.com/2009/09/23/micro-bursting-and-storage-performance/), I've read that post multiple times, and every time I read it I get something else out of it, and I'm reminded of just how much I have yet to learn. Time to get back out of that comfort zone!

* Oh, and speaking of Chad's blog...[this post](http://virtualgeek.typepad.com/virtual_geek/2009/10/howto-use-site-recovery-manager-and-linked-clones-together.html) is handy, too.

That's all for now, folks. Stay tuned for the next installation, where I'll once again share a collection of links about virtualization. Until then, feel free to share your own links in the comments.

[1]: {{< relref "2009-10-23-cutting-yourself-on-the-double-edged-sword.md" >}}
[2]: {{< relref "2006-12-04-esx-server-nic-teaming-and-vlan-trunking.md" >}}
