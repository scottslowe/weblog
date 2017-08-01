---
author: slowe
categories: News
comments: true
date: 2008-02-11T16:24:55Z
slug: xenserver-taking-advantage-of-netapp-functionality
tags:
- Citrix
- NetApp
- ONTAP
- Storage
- Virtualization
- Xen
title: XenServer Taking Advantage of NetApp Functionality
url: /2008/02/11/xenserver-taking-advantage-of-netapp-functionality/
wordpress_id: 630
---

[Via Nick](http://blogs.netapp.com/storage_nuts_n_bolts/2008/02/citrix-xenserve.html), it looks like the next version of Citrix XenServer (version 4.1, currently in beta) will have a new feature called Storage Delivery Services that is designed to take full advantage of some of the features of NetApp storage systems. Quoting from the Citrix [datasheet](http://www.citrix.com/site/resources/dynamic/partnerDocs/Adapter_datasheet.pdf):

>The adapter uses native NetApp APIs to implement XenAPI storage operations that provide access to hardware provisioning and snapshotting directly, without the proprietary layer that complicates management in other products.

To put that in normal English that people can understand, Citrix has written an "adapter" that talks directly to Data ONTAP to enable XenServer to take full advantage of functionality such as Snapshots, FlexVols, and FlexClones directly from Xen management tools. This allows Xen management tools that use the XenAPI to provision VMs using cloning functionality in the storage hardware, for example. And it can do that without some of the drawbacks that come from the use of storage system functionality to provision VMs (I describe some [advantages][1] and [disadvantages][2] here and here).

This is pretty significant, in my mind, and definitely gives Citrix XenServer a hand up on VMware who, so far, has continued to remain storage agnostic. Of course, by remaining storage agnostic, they've also chosen to forgo the benefits of tying directly into the storage hardware.

Virtualization is about using your hardware more effectively. That should include your storage as well.

[1]: {{< relref "2007-05-15-netapp-flexclones-with-vmware-part-1.md" >}}
[2]: {{< relref "2007-05-17-netapp-flexclones-with-vmware-part-2.md" >}}
