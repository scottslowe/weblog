---
author: slowe
categories: News
comments: true
date: 2009-07-22T11:50:56Z
slug: new-version-of-esxpress-supports-vsphere
tags:
- Backup
- Deduplication
- Virtualization
- VMware
- vSphere
title: New Version of esXpress Supports vSphere
url: /2009/07/22/new-version-of-esxpress-supports-vsphere/
wordpress_id: 1479
---

PHD Virtual Technologies, maker of the esXpress backup solution, are today announcing the availability of the latest version of their backup product. esXpress version 3.6 offers support for VMware vSphere 4 and, according to PHD Virtual, also offers improvements for all versions of VMware ESX since version 3.0.2.

esXpress 3.6 features a new deduplication engine that provides substantial new performance levels, based on information provided by PHD Virtual. For example:

* File-level restores are now up to four times faster  
* Image-level restores are up to twice as fast  
* Initial backups are up to twice as fast

esXpress performs source-side deduplication at the block level to help reduce backup storage requirements and help keep WAN traffic to a minimum when backing up over a WAN.

It's unclear to me whether esXpress leverages any new vSphere-specific features, like Changed Block Tracking, in the new release. PHD Virtual has committed to providing a review copy for me to run in the lab; once I've had the chance to "kick the tires," so to speak, I'll post more information here.

More details are available from [PHD Virtual's web site](http://www.phdvirtual.com/products/esxpress-virtual-backup).
