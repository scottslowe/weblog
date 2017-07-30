---
author: slowe
categories: Rant
comments: true
date: 2007-01-22T23:09:55Z
slug: vmware-ha-in-action
tags:
- Virtualization
- VMware
- VMwareHA
title: VMware HA in Action
url: /2007/01/22/vmware-ha-in-action/
wordpress_id: 405
---

[VMware](http://www.vmware.com/) touts [VMware HA](http://www.vmware.com/products/vi/vc/ha.html) as a means of providing failover protection for virtual machines:

>VMware HA is a feature-rich product that continuously monitors all physical servers in a resource pool and restarts virtual machines affected by server failure.

Well, that's all well and good, but does it actually work? I'm here to tell you, "Yes, it does."

This isn't as striking an example as running mission critical apps for a multimillion dollar venture and VMware HA saving the day, but it is an excellent example, in my mind, of how well VMware HA works. It works so well, in fact, that you _may not even notice it working_.

In my lab at the office, I have VMware HA and [VMware DRS](http://www.vmware.com/products/vi/vc/drs.html) running on a cluster containing two hosts running [ESX Server](http://www.vmware.com/products/vi/esx/). On that foundation runs a number of VMs providing various services to the lab. I had been out for a few days working on-site with customers, and when I came back into the lab this morning everything seemed fine. It wasn't until later in the day, when I finally launched the Virtual Infrastructure (VI) Client and connected to [VirtualCenter](http://www.vmware.com/products/vi/vc/), that I realized one of my ESX hosts had crashed. VMware HA had stepped in and automatically restarted all the VMs on the second host, and the other engineers who were there when the failure happened said they didn't even notice the failover. Had I not logged in to the VI Client, I wouldn't have even known that I had a host failure. It was that seamless.

&lt;aside&gt;Yes, a proper monitoring solution would have alerted me right away to this failure, and it could have been rectified much sooner. I know that. But this is a lab, not a production environment. Besides, the point here is that the lab's virtual infrastructure was resilient enough to sustain a hardware failure and continue providing services, not to berate me for the lack of a monitoring solution in the lab.&lt;/aside&gt;

What's more, after I resolved the issue with the dead ESX host and brought it back online, VMware DRS kicked in and automatically rebalanced the VM load across the two hosts, just as advertised.

Is that cool, or what?
