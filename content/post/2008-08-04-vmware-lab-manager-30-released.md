---
author: slowe
categories: News
comments: true
date: 2008-08-04T11:32:29Z
slug: vmware-lab-manager-30-released
tags:
- ESX
- Virtualization
- VMotion
- VMware
- VMwareHA
title: VMware Lab Manager 3.0 Released
url: /2008/08/04/vmware-lab-manager-30-released/
wordpress_id: 784
---

As has been already detailed elsewhere (such as [by Alessandro](http://www.virtualization.info/2008/08/release-vmware-lab-manager-30.html) or [by David](http://vmblog.com/archive/2008/08/04/new-release-of-vmware-lab-manager-enables-multiple-teams-and-projects-to-effectively-share-a-single-global-virtual-lab.aspx)), VMware has released version 3.0 of Lab Manager. This lab automation software, primarily targeted at software development and QA environments, now brings greater integration with VirtualCenter. While I haven't had the chance to fully review the release notes and the list of new features, my opinion is that the integration with VirtualCenter is **_huge_**.

Prior to this version, physical hosts running VMware ESX had to be configured for _either_ VirtualCenter _or_ Lab Manager, but not both. This also meant that Lab Manager couldn't take advantage of VMotion, VMware DRS, VMware HA, etc., as all these functions were managed by or configured by VirtualCenter. With that barrier now removed, software development environments can now utilize these functions in conjunction with Lab Manager, and there is no longer a need to segregate VMware ESX hosts into separate farms based on whether they were being managed by Lab Manager or by VirtualCenter.

I would expect to see the development and release of products such as Lab Manager to accelerate. These kinds of products are "value adds" to the core virtualization offering, and VMware wants to be sure to offer customers a full virtualization solution, not just a best-of-breed hypervisor. Of course, that's just my opinion, and I've certainly been wrong before.
