---
author: slowe
categories: News
comments: true
date: 2008-08-11T11:19:36Z
slug: virtual-iron-tackles-power-usage-with-new-release
tags:
- Virtualization
- VMotion
- VMwareHA
title: Virtual Iron Tackles Power Usage with New Release
url: /2008/08/11/virtual-iron-tackles-power-usage-with-new-release/
wordpress_id: 800
---

Virtual Iron, the company I termed "the Rodney Dangerfield" of virtualization because their products just don't get the attention and respect they probably deserve, is tackling power management and power utilization with a new release of Virtual Iron, version 4.4.

Version 4.4 adds a feature called LivePower, which builds upon Virtual Iron's LiveMigration and LiveCapacity features to provide the ability to reduce power usage during times of low utilization. During periods of low utilization, virtual machines will be consolidated onto a smaller subset of servers---using LiveMigration so as to prevent any service interruption---and then power down the servers. When utilization begins to go back up again, the additional servers are powered back up and virtual machines are once again moved and rebalanced using LiveMigration and LiveCapacity.

The addition of this functionality brings Virtual Iron almost to complete feature par with industry leader VMware:

VMware VMotion = Virtual Iron LiveMigration  

VMware Distributed Resource Scheduling (DRS) = Virtual Iron LiveCapacity  

VMware High Availability (HA) = Virtual Iron LiveRecovery  

VMware Distributed Power Management (DPM) = Virtual Iron LivePower

VMware still has the feature lead with other features like Storage VMotion, but Virtual Iron is a close second. In my mind, they are a more serious competitor to VMware than Citrix (Virtual Iron seems much more robust and mature than XenServer).

Full information on the new release and its new power management functionality can be found on [Virtual Iron's web site](http://www.virtualiron.com/). The full press release is available [here](http://www.virtualiron.com/News-and-Events/News-Releases/index.php?prId=91).
