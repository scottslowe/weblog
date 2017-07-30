---
author: slowe
categories: News
comments: true
date: 2008-09-15T11:59:59Z
slug: vmwares-virtual-datacenter-os
tags:
- ESX
- ESXi
- Virtualization
- VMotion
- VMware
- VMwareHA
- VMworld2008
title: VMware's Virtual Datacenter OS
url: /2008/09/15/vmwares-virtual-datacenter-os/
wordpress_id: 895
---

So, the news is out on VMware's new announcement regarding the Virtual Datacenter OS (VDC-OS). You can find more coverage of this on a few other sites:

[Virtual Datacenter OS from VMware](http://www.vmware.com/technology/virtual-datacenter-os/)  

[VMware moving to cloud-computing with Virtual Datacenter Operating System (VDC-OS)](http://www.gabesvirtualworld.com/?p=84)  

[VMware testing data center OS for managing, literally, everything](http://www.computerworld.com/action/article.do?command=viewArticleBasic&articleId=9114679)  

[VMware Tries to Expand Throughout the Data Center](http://www.pcworld.com/businesscenter/article/151067/vmware_tries_to_expand_throughout_the_data_center.html)

I'm kind of excited to be able to talk about this, because it was a topic of the Partner Technical Advisory Board today and I didn't think I'd be able to talk about it until later in the week.

First off, let me just state that the Computerworld article linked above ("VMware testing data center OS for managing, literally, everything") is just plain **wrong**. What VMware is working on is an OS for the virtual datacenter, not a virtual OS for the datacenter. The distinction is important. VDC-OS isn't intended to be an OS for all aspects of the datacenter. It's intended to be an OS for all aspects of the _virtual datacenter_.

When you think of an OS, you think of software that manages access to resources and provides services to applications. That's what VMware is doing with VDC-OS: managing access to resources and providing services to applications, only this time the applications are workloads (virtual machines with an OS and a set of applications on that guest OS). VDC-OS will provide sets of services to these applications:

* Application vServices, like availability, security, and scalability. These application vServices are provided via features like VMotion, Storage VMotion, VMware HA, VCB, and---in the future---stuff like VMware Fault Tolerance (FT), formerly known as Continuous Availability. See [this page](http://www.vmware.com/technology/virtual-datacenter-os/application.html) on VMware's site for more examples.

* [Infrastructure vServices](http://www.vmware.com/technology/virtual-datacenter-os/infastructure.html), like compute functionality (vCompute), networking connectivity (vNetwork), or storage features (vStorage). These vServices are manifested as features like VMware DRS, and will be extended in the future with things like vStorage Linked Clones, or 3rd party virtual switches, or VMDirectPath. The APIs are there for additional third party vServices to be added as well; one example would be network load balancing as an infrastructure vService.

* Cloud vServices enable the interaction of on-premise infrastructure (the servers in your data center) to integrate with external cloud infrastructure. There are no concrete examples to really share here; in my opinion, this is the most nebulous part of this announcement. See [this page](http://www.vmware.com/technology/virtual-datacenter-os/cloud-vservices/) for more information.

* Finally, management vServices provide...well, management functionality for the virtual data center and the applications running in the virtual data center. More information is available [here](http://www.vmware.com/technology/virtual-datacenter-os/simplified_management.html).

The way to really view VDC-OS is not as a "datacenter OS", because it's not intended to provide automation of non-virtual resources. Instead, look at VDC-OS as a framework. Within this framework are sets of services that can be extended or modified in very standardized ways (via APIs and SDKs) to provide different functionality for the applications running within that framework. Some third party ISV wants to write a different way of providing fault tolerance and failover? Fine, no problem, that can be plugged into the VDC-OS application vService framework for availability. I used the example earlier of a load balancer as an infrastructure vService. VMware announced the VMsafe APIs back at VMworld Europe, and the idea of VMsafe ties directly into API access for ISVs to develop new or different security-related application vServices that can be provided to all applications running within the VDC-OS, such as anti-virus or host-based intrustion detection/prevention.

I was a bit worried that this messaging wasn't going to be received as clearly as I had hoped it would be, and the initial coverage I'm seeing so far confirms that many people are going to misread what VMware is trying to do. Hopefully, we can get the idea across to everyone so that they can begin to really understand where VMware is headed with this idea and why it is the next step in the evolution and maturation of virtualization.
