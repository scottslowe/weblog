---
author: slowe
categories: News
comments: true
date: 2008-09-08T14:48:51Z
slug: sanbolic-now-providing-shared-lun-access-for-hyper-v-guests
tags:
- ESX
- HyperV
- Microsoft
- SAN
- Storage
- Virtualization
- VMware
title: Sanbolic Now Providing Shared LUN Access for Hyper-V Guests
url: /2008/09/08/sanbolic-now-providing-shared-lun-access-for-hyper-v-guests/
wordpress_id: 872
---

Sanbolic, whose [Melio FS product I first mentioned back in June][1], has today announced that they now support the use of Melio FS with Hyper-V guest virtual machines. Their previous announcement, back in June during Tech-Ed 2008, centered around the use of Melio FS with Hyper-V hosts and VMware ESX guests.

This announcement now makes Melio FS supported in a variety of environments and use cases:

* Users can run Melio FS on Hyper-V **_hosts_** to eliminate the "one LUN, one VM" rule imposed by Quick Migration. This is an NTFS limitation that is removed by the use of Melio FS on the Hyper-V hosts.

* Users can run Melio FS in Hyper-V **_guest VMs_** to provide shared LUN access for multiple VMs. This is an NTFS limitation that is addressed by Melio FS.

* Users can run Melio FS in VMware ESX **_guest VMs_** to provide shared LUN access to multiple VMs. This is an NTFS limitation that is addressed by Melio FS.

Today's announcement mirrors the announcement in June, where Sanbolic announced support for running Melio FS in VMware ESX guest VMs. Today announcement adds support for running Melio FS in Hyper-V guest VMs as well.

The full press release is available [here](http://www.sanbolic.com/pdfs/Sanbolic_Press_Release_Hyper-V_Guest_Support_Final.pdf).

[1]: {{< relref "2008-06-16-melio-fs-hyper-v-and-vmware-esx.md" >}}
