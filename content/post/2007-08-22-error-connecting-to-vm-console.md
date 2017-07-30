---
author: slowe
categories: Explanation
comments: true
date: 2007-08-22T13:40:20Z
slug: error-connecting-to-vm-console
tags:
- ESX
- Networking
- Virtualization
- VMware
title: Error Connecting to VM Console
url: /2007/08/22/error-connecting-to-vm-console/
wordpress_id: 508
---

During an upgrade of a server running [ESX Server](http://www.vmware.com/products/vi/esx/) 3.0.0 to ESX Server 3.0.2, we also moved the server to a new server room on a new subnet. The upgrade itself was uneventful and took only a few minutes (as I had expected), but what happened afterward caught me a little off-guard, as did the eventual solution.

We needed to change the IP address of the service console, so after the upgrade was complete I simply edited the `/etc/sysconfig/network-scripts/ifcfg-vswif0` file to include the new IP address, restarted networking, and went on about my way. Everything seemed fine; the ESX host responded across the network, responded properly within [VirtualCenter](http://www.vmware.com/products/vi/vc/), powered on the VMs, etc. In hindsight, I probably should have used the `esxcfg-vswif` command instead of editing the configuration file directly, but as they say, "Hindsight has 20/20 vision."

It wasn't until a few minutes later that we realized we were unable to connect to any VM's console. When we tried to open a VM's console, we received an error message to the effect that the "host had responded incorrectly". Strangely enough, this problem only seemed to affect VI client installations; we were able to connect without any problems from the VirtualCenter server itself.

Thinking that perhaps we had run into an ACL on one of the network switches, I tried opening a telnet connection to TCP port 902 on the VirtualCenter server. That worked just fine, so that eliminated the possibility of a router/switch ACL blocking the traffic, and also eliminated the possibility that a host-based firewall on the VirtualCenter server was causing the problem. (A second review a couple minutes later verified again that Windows Firewall was not running and therefore could not be the problem.) It wasn't DNS name resolution; both the VC server and VI clients were able to resolve the hostnames of all the ESX servers as well as the individual guest VMs.

"Aha!" I thought. "I need to restart mgmt-vmware because I didn't restart that service after changing the IP address." Alas, that didn't work either.

Finally, a Google search turned up [this thread](http://www.vmware.com/community/message.jspa?messageID=613061) and [this thread](http://www.vmware.com/community/thread.jspa?threadID=64422&tstart=120&messageID=550656) from the VMTN Community Forums, both of which referenced the `/root/anaconda-ks.cfg` file. An Anaconda kickstart file causing the problem? It didn't make any sense to me, but just for kicks I made the following changes:

* Edited the `/root/anaconda-ks.cfg` file to show the correct IP addressing information for the Service Console

* Edited the `/etc/sysconfig/network` file to have the right gateway IP address (strangely enough, the Service Console seemed to be routing traffic correctly even with an incorrect gateway IP address)

* Restarted networking and the mgmt-vmware services

Lo and behold, the VM consoles now worked perfectly. I'm still not sure which of the changes actually corrected the problem; I hope to be able to try to recreate this problem in the lab and more closely determine what the exact cause and resolution were. When I have some additional information, I'll post it here.

Anyone else run into this problem?
