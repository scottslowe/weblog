---
author: slowe
categories: Information
comments: false
date: 2006-01-24T14:21:33Z
slug: gsx-upgrade-much-smoother-this-time
tags:
- CentOS
- Linux
- Networking
- RedHat
- Virtualization
- VMware
- Windows
title: GSX Upgrade Much Smoother This Time
url: /2006/01/24/gsx-upgrade-much-smoother-this-time/
wordpress_id: 166
---

Earlier today I completed another [GSX Server](http://www.vmware.com/products/gsx/) upgrade (from version 2.5 to version 3.2.1) for a customer, and fortunately this upgrade was much smoother than [the last GSX Server upgrade][1].

No BSODs (Blue Screens of Death) this time during the uninstallation, and the previous version uninstalled itself cleanly. The installation of the new version went quickly and smoothly, and in practically no time we were booting up "legacy" VMs under the new version of GSX Server.

As with the previous upgrade, those VMs running Red Hat Linux 9.0 detected "new" hardware (specifically, a "new" network card and a "new" SCSI card; ironically, they are exactly the same virtual hardware as before) and seamlessly migrated over the configuration without any issues whatsoever.

Unfortunately, I was dismayed to find that a VM running CentOS 4.2 did not maintain the network configuration while setting up this "new" hardware, so I had to go back in and re-enter the network settings. This was only a minor inconvenience, as the network reconfiguration was quick and easy. It's also nice to note that the [time synchronization problems with CentOS 4.2][2] appear to have resolved themselves now under the new version of GSX Server. (Note that even after [the changes that mostly resolved the NTPd problems][3], time was still slightly off and lots of NTPd messages were being logged; even those issues have disappeared as well.)

The network reconfiguration on a VM running Windows Server 2003, on the other hand, was _not_ quick and easy. As before, the network configuration simply disappeared during the discovery of "new" hardware. In the last upgrade for this customer, the only Windows-based VM we had to work with was an older Windows 2000-based server. I had hoped that the problems we'd seen with that server would be resolved in Windows Server 2003.

Not so. The VMware Tools installation removed the driver and installed a new driver, so even if the network configuration had made it through the "new hardware" discovery process, it would have been hosed at that point. Eventually, after a couple of different reboots, I finally had the Windows server up and running with its original network configuration again. If there is one area I've found so far that VMware really needs to work on, it's this one.

[1]: {{< relref "2006-01-10-gsx-server-upgrade-woes.md" >}}
[2]: {{< relref "2005-12-19-ntpd-on-centos-42.md" >}}
[3]: {{< relref "2005-12-23-centos-ntpd-problem-mostly-resolved.md" >}}
