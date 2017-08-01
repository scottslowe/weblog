---
author: slowe
categories: Tutorial
comments: true
date: 2008-09-05T11:14:49Z
slug: setting-vmware-esx-vswitch-load-balancing-policy-via-cli
tags:
- ESX
- Networking
- Virtualization
- VMware
- CLI
title: Setting VMware ESX vSwitch Load Balancing Policy via CLI
url: /2008/09/05/setting-vmware-esx-vswitch-load-balancing-policy-via-cli/
wordpress_id: 861
---

I see this question popping up a lot, so I thought I'd just throw up this quick blog entry with the command that's necessary to set the load balancing policy for a VMware ESX vSwitch.

In VMware ESX 3.5 U2 (which users should be using if at all possible, now that [it's validated by Microsoft][1]), the command to do this is vmware-vim-cmd:

	vmware-vim-cmd /hostsvc/net/vswitch_setpolicy --nicteaming-policy=loadbalance_ip vSwitch1

This command sets the vSwitch to use "Route based on ip hash". To set the vSwitch back to "Route based on the originating virtual port ID", use this command:

	vmware-vim-cmd /hostsvc/net/vswitch_setpolicy  --nicteaming-policy=loadbalance_srcid vSwitch1

Obviously, users will need to replace vSwitch1 with the appropriate vSwitch that needs to be configured. Note that this command is a bit different than in earlier versions, which used `vimsh`.

I hope this is useful!

[1]: {{< relref "2008-09-03-vmware-esx-35-u2-validated-via-svvp.md" >}}
