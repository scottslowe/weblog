---
author: slowe
categories: Explanation
comments: true
date: 2013-11-01T09:00:00Z
slug: learning-nvp-part-7-handling-the-nvp-to-nsx-transition
tags:
- Networking
- Nicira
- NSX
- NVP
- Virtualization
title: 'Learning NVP, Part 7: Handling the NVP to NSX Transition'
url: /2013/11/01/learning-nvp-part-7-handling-the-nvp-to-nsx-transition/
wordpress_id: 3322
---

Welcome to part 7 of the Learning NVP blog series, in which I will discuss transitioning from a focus on NVP to looking at NSX.

If you're just now joining me for this series, here's what's transpired thus far:

* In part 1, I provided a [high-level overview of NVP][1] and its core components.

* In part 2, I showed you how to [build NVP controllers and configure them into a controller cluster][2].

* In part 3, you saw how to [install and configure NVP Manager][3], a web-based GUI that you can use to configure certain aspects of NVP.

* In part 4, I walked you through the process of [adding hypervisors to NVP][4].

* In part 5, I showed you how to [create a logical network][5] that could be used to connect VMs to each other independent of the underlying physical network topology.

* In part 6, I took you through [adding an NVP gateway][6] to your environment, setting the stage for you to later add L2/L3 connectivity in and out of your logical networks.

When I first started this series back in May of this year, I said this:

>Before continuing, it might be useful to set some context around NVP and NSX The architecture I'm describing here will also be applicable to NSX, which VMware announced in early March. Because NSX will leverage NVP's architecture, spending some time with NVP now will pay off with NSX later.

Well, the "later" that I referenced is now upon us. I had hoped to be much farther along with this blog series by now, but it has proven more difficult than I had anticipated to get this content written and published. Given that NSX officially GA'd last week at VMworld EMEA in Barcelona, I figured it was time to make the transition from NVP to NSX.

The way I'll handle the transition from talking NVP to discussing VMware NSX is through an upgrade. I have a completely virtualized environment that is currently running all the NVP components: three controllers, NVP Manager, three nested hypervisors running Ubuntu+KVM+OVS, two gateways, and a service node. (I know, I know---I haven't written about service nodes yet. Sorry.) The idea is to take you through the upgrade process, upgrading my environment from NVP 3.1.1 to NVP 3.2.1 and then to NSX 4.0.0. From that point forward, the series will change from "Learning NVP" to "Learning NSX", and I'll continue with discussing all the topics that I have planned. These include (among others):

* Deploying service nodes

* Using an L2 gateway service

* Using an L3 gateway service

* Enabling distributed east-west routing

* Many, many more topics

Unfortunately, my travel schedule over the next few weeks is pretty hectic, which will probably limit my ability to move quickly on performing and documenting the upgrade process. Nevertheless, I will press forward as quickly as possible, so stay tuned to the site for more updates as soon as I'm able to get them published.

Questions? Comments? Feel free to add them below. All I ask for is common courtesy and disclosure of vendor affiliations, where applicable. Thanks!

[1]: {{< relref "2013-05-21-learning-nvp-part-1-high-level-architecture.md" >}}
[2]: {{< relref "2013-08-16-learning-nvp-part-2-nvp-controllers.md" >}}
[3]: {{< relref "2013-08-19-learning-nvp-part-3-nvp-manager.md" >}}
[4]: {{< relref "2013-08-22-learning-nvp-part-4-adding-hypervisors-to-nvp.md" >}}
[5]: {{< relref "2013-09-06-learning-nvp-part-5-creating-a-logical-network.md" >}}
[6]: {{< relref "2013-10-28-learning-nvp-part-6-adding-an-nvp-gateway.md" >}}
