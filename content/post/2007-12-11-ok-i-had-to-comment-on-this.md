---
author: slowe
categories: Rant
comments: true
date: 2007-12-11T23:21:35Z
slug: ok-i-had-to-comment-on-this
tags:
- ESX
- Microsoft
- Virtualization
- VMware
title: OK, I Had to Comment on This
url: /2007/12/11/ok-i-had-to-comment-on-this/
wordpress_id: 592
---

Naturally, [an article](http://blogs.technet.com/doxley/archive/2007/12/10/virtualisation-why-would-you-not-choose-microsoft-virtual-server.aspx) titled "Why would you not choose Microsoft Virtual Server...?" is going to grab my attention. Microsoft Virtual Server is a decent enough product in the hosted virtualization space, but certainly doesn't compete with VMware ESX Server or other solutions based on a Type 1 bare metal hypervisor.

Apparently, [this image](http://blogs.technet.com/blogfiles/doxley/WindowsLiveWriter/VirtualisationWhywouldyounotchooseMicro_D41B/costs_2.jpg) sums up the argument _against_ VMware Infrastructure 3 and _for_ Microsoft Virtual Server. Go look at the image, then come back and finish reading this article.

OK, done now? Good. Let's take a closer look at the information presented in this article:

* On the line listed "Migration," there's a checkmark in the column for the Microsoft solution. Since Microsoft's solution---today in the form of Virtual Server, late next year in the form of Hyper-V---[lacks any form of live migration][1] (and you [can't count "Quick Migration"][2]), this table must be referring to cold migrations. That being the case, then you don't need the extra charges listed in the first column.

* If that is not the case and we _are_ referring to live migration functionality, then the checkmark needs to be removed from Microsoft's column. And, I would ask you this question: how much is it worth to be able to move a workload from one physical host to another physical host during the midst of the workday with no interruption in service?

* Microsoft's solution lacks any form of multi-chassis dynamic resource management, so that line must be referring to in-chassis resource management. All versions of VMware products, from VMware Server to VMware Workstation to VMware Fusion to VMware Infrastructure 3, have resource management abilities. On the other hand, if we _are_ referring to the ability to dynamically distribute workloads across multiple physical hosts, then the checkmark in Microsoft's column must be removed. Microsoft's solution doesn't have that ability. How do you quantify the ability to have all your workloads distributed evenly across the physical hosts---dynamically?

* What about the things that aren't listed on this table, like transparent page sharing? Or some of the features in VI3 3.5 like Distributed Power Management (DPM)? How do you quantify the value of these types of features?

Don't get me wrong, Microsoft Virtual Server is a decent solution at a great price---free, last time I checked. Of course, VMware Server is also a free hosted virtualization product. But as I've said a million times already, you can't compare Virtual Server and ESX Server. They are two different classes of products, with different feature sets and intended at different markets. When Hyper-V finally makes its debut late next year, then we can really discuss the merits of Microsoft's hypervisor-based solution against the other hypervisor-based solutions, including VMware, that are available on the market.

Until then, articles such as [this one](http://blogs.technet.com/doxley/archive/2007/12/10/virtualisation-why-would-you-not-choose-microsoft-virtual-server.aspx) are, in my humble opinion, useless.

[1]: {{< relref "2007-05-10-viridian-feature-set-trimmed.md" >}}
[2]: {{< relref "2007-07-23-live-migration-vs-quick-migration.md" >}}
