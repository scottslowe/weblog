---
author: slowe
categories: Information
comments: false
date: 2006-04-27T11:25:36Z
slug: centos-on-esx-server
tags:
- CentOS
- ESX
- Linux
- OSS
- Virtualization
- VMware
title: CentOS on ESX Server
url: /2006/04/27/centos-on-esx-server/
wordpress_id: 234
---

I'm happy to report that [CentOS](http://www.centos.org/) 4.3 appears to run just fine on [ESX Server](http://www.vmware.com/products/esx/) 2.5.3. I built a CentOS server in the lab today for additional testing on the Linux-AD integration instructions with [Windows Server 2003 R2](http://www.microsoft.com/windowsserver2003/), and found that CentOS appears to run just fine.

The virtual machine configuration was specified as a single CPU (I haven't tested it with Virtual SMP) with the vlance virtual NIC and the LSI Logic SCSI adapter.

In the past I experienced problems with time synchronization inside CentOS when running as a virtual machine (described [here][1] and [here][2]). As I have not yet had the time to test time synchronization, I don't know if the problem will crop up again.

[1]: {{< relref "2005-08-16-strange-ntpd-problem-on-centos-41.md" >}}
[2]: {{< relref "2005-12-19-ntpd-on-centos-42.md" >}}
