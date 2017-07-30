---
author: slowe
categories: Rant
comments: false
date: 2006-04-09T14:21:30Z
slug: open-virtual-disk-formats
tags:
- Microsoft
- Virtualization
- VMware
title: Open Virtual Disk Formats
url: /2006/04/09/open-virtual-disk-formats/
wordpress_id: 224
---

[VMware](http://www.vmware.com/) has [announced](http://www.vmware.com/news/releases/vmdk.html) that it is making its virtual machine disk (VMDK) format openly available, downloadable and free of charge. This introduction sets the stage for a battle for open virtual disk formats, with the major players being VMware and [Microsoft](http://www.microsoft.com/).

VMware, being the market leader, seeks to continue its leadership position by fostering the creation of a thriving third-party market for add-ons to its core products. By opening its VMDK format, VMware allows other companies to create add-ons to its virtualization products. As the number of third-party companies creating add-ons grows, the customer base grows and attracts more developers, etc. This self-sustaining community then helps drive adoption of VMware's products.

On the flip side, however, software behemoth Microsoft is licensing its virtual hard disk (VHD) format to other companies. Surprisingly enough, one of the companies to [adopt the VHD format](http://www.crn.com/sections/breakingnews/breakingnews.jhtml;jsessionid=AEBZUBHQDWZPAQSNDBECKHSCJUMEKJVN?articleId=184425640) was [XenSource](http://www.xensource.com/), the commercial company behind the development of the open source Xen hypervisor. This creates a bit of an odd alliance---we have a group of companies that generally align themselves _against_ Microsoft ([HP](http://www.hp.com/), [IBM](http://www.ibm.com/), [Novell](http://www.novell.com/), and [Red Hat](http://www.redhat.com/), among others) adopting a virtual hard disk format, VHD, that is owned by Microsoft. It would have made more sense for these companies to adopt VMware's VMDK format (especially considering that the licensing conditions from VMware seem _much_ more open source friendly) in order to help prevent Microsoft from taking over yet another market by bundling software into Windows.
