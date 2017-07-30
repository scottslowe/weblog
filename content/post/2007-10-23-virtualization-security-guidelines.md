---
author: slowe
categories: News
comments: true
date: 2007-10-23T11:24:11Z
slug: virtualization-security-guidelines
tags:
- Security
- Virtualization
- VMware
title: Virtualization Security Guidelines
url: /2007/10/23/virtualization-security-guidelines/
wordpress_id: 564
---

The [Center for Internet Security (CIS)](http://www.cisecurity.org/) has released some security benchmarks for VMware ESX Server 3.0.x. The ESX security benchmark joins recommendations and guidelines for Windows 2000, Windows XP, Windows Server 2003, Red Hat Linux, and Mac OS X that are also available from the CIS. The CIS has also published a generic virtual machine (VM) security benchmark as well. Taken together, the ESX benchmark and the VM benchmark provide good, solid recommendations around virtualization security.

The ESX/VM benchmarks are available for download [here](http://www.cisecurity.org/bench_vm.html).

With all the hype around virtualization's impact on security---positive or negative---it's good to see some concrete and actionable recommendations. If nothing else, these documents will at least provide a starting point for security professionals and virtualization experts to discuss the best way to architect solutions without compromising the security of the network/servers. In fact, we _might_ even find ways to enhance the security of the network/servers.

I feel it's also important to mention that a number of the recommendations found in the CIS benchmark are also included in the VI3 Server Configuration Guide (available [here](http://www.vmware.com/pdf/vi3_server_config.pdf) from VMware's web site). Why is this important to know? In my opinion, VMware has done a reasonable job of making security information available. They haven't done a stellar job, but they haven't done a shoddy job either, and I believe that many of the concerns around "virtualization is insecure" derive directly from the failure of implementing parties to properly apply the knowledge that was available from the vendor. That means reseller/VAR SEs, like myself, who help customers implement this stuff are the ones responsible for making sure that we know what VMware's recommendations are and that we properly relay those to the customer(s). Of course, if the customer chooses not to follow recommendations, that's another story...

In any case, go get the CIS benchmarks, review them, and put them to work.

Thanks to Chris Hoff for [alerting everyone to the release](http://rationalsecurity.typepad.com/blog/2007/10/version-10-of-t.html) of the ESX Server benchmark.
