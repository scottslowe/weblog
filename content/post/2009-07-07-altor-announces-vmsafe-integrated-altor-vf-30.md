---
author: slowe
categories: News
comments: true
date: 2009-07-07T18:10:16Z
slug: altor-announces-vmsafe-integrated-altor-vf-30
tags:
- Security
- Virtualization
- VMware
title: Altor Announces VMsafe-Integrated Altor VF 3.0
url: /2009/07/07/altor-announces-vmsafe-integrated-altor-vf-30/
wordpress_id: 1448
---

Since the announcement of the VMsafe APIs at VMworld Europe 2008, the virtualization world has been waiting. First, we waited for the actual release of the VMsafe APIs, which came with the release of VMware vSphere 4. Next, we waited for the delivery of the first VMsafe-integrated security solutions. While I can't say definitively that it's the first, Altor Networks is announcing its VMsafe-integrated virtual firewall solution, Altor VF 3.0. The wait is over, and now we get to see: just how powerful does VMsafe allow virtual security solutions to be?

Only time will provide the full picture, but an initial glance at Altor's press release and a pre-release discussion I had with Altor lead me to believe that VMsafe really _will_ change the landscape of security solutions in VMware environments. By leveraging VMsafe in fast-path mode---meaning that the security solution runs as a module in the hypervisor---Altor is able to provide not only firewalling functionality but also intrusion detection functionality as well. In fact, the intrusion detection features can be configured to work only on traffic that successfully passes through the firewall rules.

Altor also claims much greater performance with Altor VF 3.0, up to ten times the performance of a virtual machine-based security solution. And, of course, Altor has ensured that their virtual firewall product can apply firewall rules at various levels within the VMware vCenter Server hierarchy, and the product also helps protect the hypervisor management interfaces as well (the Service Console interfaces in ESX, Management interfaces in ESXi).

The initial release of Altor VF 3.0 will use a separate web-based management console, but Altor Networks did indicate that they are investigating the use of a plug-in for the vSphere Client for more integrated management. Future versions of Altor VF also plan to address vApp integration, something that is missing from the initial release.

For more detailed information or for the [full press release](http://www.altornetworks.com/news-events/item.php?pressrel-altor-unveils-vf3), visit [Altor Networks' web site](http://www.altornetworks.com/).
