---
author: slowe
categories: Explanation
comments: true
date: 2007-05-17T09:49:34Z
slug: netapp-flexclones-with-vmware-part-2
tags:
- NetApp
- Storage
- Virtualization
- VMware
title: NetApp FlexClones with VMware, Part 2
url: /2007/05/17/netapp-flexclones-with-vmware-part-2/
wordpress_id: 457
---

[Part 1][1] in our series on [NetApp FlexClones](http://www.netapp.com/products/enterprise-software/storage-system-software/provisioning-volume-management/flexclone.html) and [VMware](http://www.vmware.com/) discussed in greater detail some of the advantages of using FlexClones for VM provisioning. In that article, we saw that using FlexClones can greatly reduce both the storage required for new VMs as well as the time required to provision new VMs, especially when the storage needed by the VMs is large. Both of these advantages can be very compelling.

However, in order to make an informed decision about whether we should use FlexClones we must also look at the disadvantages of this approach. In this part of the series, we'll take a look at some of those disadvantages.

Just as there were two key advantages to using FlexClones, there are also two key disadvantages to using FlexClones:

* First, the use of FlexClones (unless properly architected) may cause installations to bump up against the maximum number of LUN IDs available (255), or the maximum number of LUNs that may be opened concurrently by all virtual machines (256). (Both of those numbers were taken from the "Configuration Maximums for VMware Infrastructure 3" white paper, available [here](http://www.vmware.com/pdf/vi3_301_201_config_max.pdf).) Unless properly architected, this means that your installation could be limited to about 250 virtual machines. Note that there are ways around this limitation; see below.

* Second, there is currently no integration from either [Network Appliance](http://www.netapp.com/) or VMware to help automate the process of using FlexClones for VM provisioning. As shown in my technical article on [how to use FlexClones for VM provisioning][2], there are a number of manual steps currently, and users seeking a greater level of automation must create their own scripts or hope that someone else already has written scripts they can re-use.

While these are the biggest disadvantages (in my opinion) there are also a number of other, smaller disadvantages as well:

* Using FlexClones for VM provisioning blurs the operational responsibilities of the SAN administrators and the server administrators; SAN administrators are now responsible for provisioning new VMs via FlexClones.

* Using FlexClones for VM provisioning adds complexity to the solution, making it harder to troubleshoot and administer over time.

* Making the most of the FlexClone space-conserving functionality requires more specialized VM configuration, such as dedicated VMDKs for pagefiles/swap partitions on separate LUNs.

These limitations can really derail the use of FlexClones in larger deployments. Ironically, it's the larger deployments where the advantages of FlexClones are the most significant. In larger environments, there are typically dedicated SAN administrators and dedicated VMware/VI3 administrators. Introducing the use of FlexClones in this type of environment now begs the question: who's responsible for provisioning VMs?

While some of these disadvantages are inherent in the solution, there is a workaround for one of them, and that's the maximum number of LUN IDs/LUNs. Because each FlexClone is a separate LUN with a separate LUN ID, your installation will be limited to about 250 LUNs. If you only place one VM per LUN, then you'll be limited to about 250 VMs. However, if we instead take the practice of placing multiple VMs---each of them prepared for cloning as described in my how-to article---then we can easily scale the number of VMs. For example, we could create a master VMFS datastore that had 10 VMs built and prepared for cloning, then deploy FlexClones for each group of 10 VMs that were needed. Using this technique, it would be reasonably easy to scale this solution to support 2,500 VMs (250 VMFS datastores with 10 VMs each). (Of course, 1,500 VMs is the configuration maximum for a single VirtualCenter management server.)

In summary, the use of FlexClones for VMware environments has some very compelling advantages, and some very significant disadvantages. It will be up to each organization to weigh these pros and cons against the specifics of their company to determine which route to take. At least now you have some information upon which to base your decision.

As always, I welcome any comments below.

[1]: {{< relref "2007-05-15-netapp-flexclones-with-vmware-part-1.md" >}}
[2]: {{< relref "2007-05-11-how-to-provision-vms-using-netapp-flexclones.md" >}}
