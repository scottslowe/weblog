---
author: slowe
categories: Information
comments: true
date: 2008-09-25T11:34:04Z
slug: a-collection-of-hyper-v-links
tags:
- HyperV
- Microsoft
- Virtualization
- Windows
title: A Collection of Hyper-V Links
url: /2008/09/25/a-collection-of-hyper-v-links/
wordpress_id: 953
---

While trying to clear out the backlog of articles that have accumulated in my Reading/Review context in OmniFocus, I've come across a number of Hyper-V articles on various topics:

* Robert Larson has a good article at VirtualizationAdmin.com that covers [how to work with VLANs with Hyper-V](http://www.virtualizationadmin.com/articles-tutorials/microsoft-hyper-v-articles/networking/dealing-mac-address-pool-duplication-hyperv.html). That includes information on configuring the parent partition to use VLANs as well as configuring child partitions to use VLANs. The title of the article is completely wrong for the content, but it's still a useful article nevertheless.

* [Via Ben Armstrong](http://blogs.msdn.com/virtual_pc_guy/archive/2008/09/18/vmc-to-hyper-v-import-tool.aspx), I saw that there is [a VMC to Hyper-V import tool](http://blogs.technet.com/matthts/archive/2008/09/12/vmc-to-hyper-v-import-tool-available.aspx). More information on the tool is available here. In addition, Ben also recently mentioned [a Hyper-V VSS hotfix](http://blogs.msdn.com/virtual_pc_guy/archive/2008/09/23/hyper-v-vss-hotfix-available.aspx) that fixes a problem with VSS failing to backup any VMs if even a single VM has a corrupt or invalid configuration file.

* And while we're on the subject of migrating to Hyper-V from Virtual Server or Virtual PC, [this blog post](http://blogs.technet.com/askcore/archive/2008/09/19/migrating-to-hyper-v-from-virtual-server-or-virtual-pc-tips-and-suggestions.aspx) provides a checklist of things to do when migrating virtual machines from those older platforms to Hyper-V.

* Hyper-V's Linux Integration Components---the paravirtualized drivers (or synthetic devices, or virtualization-optimized components, or whatever else you want to call them) that provide better performance under Hyper-V---have been [officially released](http://blogs.msdn.com/virtual_pc_guy/archive/2008/09/22/rtm-of-linux-integration-components-for-hyper-v-now-available.aspx).

* By the way, [this page](http://www.microsoft.com/servers/hyper-v-server/default.mspx) provides a comparison of Hyper-V Server 2008 and more "traditional" implementations of Hyper-V with a full Windows Server 2008 parent partition.

* Looking for a comparison of performance with dynamic VHDs and fixed VHDs? Inquiring minds want to know! Get the scoop [here](http://blogs.technet.com/winserverperformance/archive/2008/09/19/hyper-v-and-vhd-performance-dynamic-vs-fixed.aspx).

That's it for now. If any readers have other useful or helpful Hyper-V links, please share them in the comments.
