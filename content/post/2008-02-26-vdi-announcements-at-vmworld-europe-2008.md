---
author: slowe
categories: News
comments: true
date: 2008-02-26T09:28:34Z
slug: vdi-announcements-at-vmworld-europe-2008
tags:
- Citrix
- NetApp
- Storage
- VDI
- Virtualization
- VMware
title: VDI Announcements at VMworld Europe 2008
url: /2008/02/26/vdi-announcements-at-vmworld-europe-2008/
wordpress_id: 644
---

Back in 2006, I [speculated][1] that one day VMware would allow hosted virtual desktops to be "checked out" and used offline. Lo and behold, one of the announcements that has come out of VMworld Europe 2008 is just that very thing (quoting from [VMware's web site](http://www.vmware.com/whatsnew/virtual-desktops.html)):

>Offline Virtual Desktop Infrastructure previews how a single virtual desktop infrastructure platform may be able to support all enterprise PCs in the future. Let end users "check out" personalized virtual desktops running on VMware virtual desktop infrastructure to a notebook computer for use offline and then "check back in" to the same desktop running in their virtual desktop infrastructure environment.

Also coming out of VMworld Europe 2008 is the announcement of linked clones technology on the VI platform:

>Scalable Virtual Image technology delivers lower operational costs through simple and scalable desktop image management and reduces storage requirements up to 90 percent for virtual desktop infrastructure environments. Quickly deploy, update, and publish desktop images to thousands of virtual machines.

This is powerful stuff. The offline VDI stuff really enables an entirely new way of thinking about VDI; it's no longer about just hosting desktops at the datacenter. Now it's about providing a "golden image" that users can run on the local machine when they're not in the office and on the server farm when they are in the office.

Likewise, the scalable virtual image stuff addresses what is, in my mind, the #1 problem with VDI deployments: storage requirements. Vendors like Network Appliance have attempted to address this through their technologies like FlexClone (like described [here][2]); competitors such as Citrix have attempted to address this problem through technologies like Citrix Provisioning Server (formerly Ardence).

With these announcements, it's now much clearer that VMware sees the desktop virtualization market is a very strategic market, and they are taking the steps to control that market.

[1]: {{< relref "2006-11-08-vmworld-2006-day-2-keynote.md" >}}
[2]: {{< relref "2007-05-11-how-to-provision-vms-using-netapp-flexclones.md" >}}
