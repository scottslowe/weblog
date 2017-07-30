---
author: slowe
categories: Explanation
comments: true
date: 2008-12-16T11:34:14Z
slug: using-vmware-vim-cmd-to-modify-a-portgroup
tags:
- ESX
- Networking
- Virtualization
- VMware
title: Using vmware-vim-cmd to Modify a Portgroup
url: /2008/12/16/using-vmware-vim-cmd-to-modify-a-portgroup/
wordpress_id: 1100
---

While creating a command-line configuration guide for a design that I'd prepared, I found myself in need of a command that sets the NIC failover order explicitly for a portgroup. I suspected---and was correct---that `vmware-vim-cmd` would do the job, but was having a hard time finding documentation on the correct syntax. Even the [extensive XtraVirt `vimsh` white paper](http://knowledge.xtravirt.com/white-papers/scripting.html) did not have information on the syntax. After a little bit of digging around, I found the information for which I was looking. Here it is for future benefit.

The syntax for using `vmware-vim-cmd` to modify the NIC failover order for a portgroup looks like this:

	vmware-vim-cmd hostsvc/net/portgroup_set --nicorderpolicy-active=<List of NICs> <vSwitch#> <Portgroup>

If the portgroup name includes spaces, then wrap the portgroup name in double quotes, like this:

	vmware-vim-cmd hostsvc/net/portgroup_set --nicorderpolicy-active=vmnic0 vSwitch0 "Service Console"

That sets only the active NICs. Any NICs not included in the command above are set to Unused, which is generally not the desired result. Usually, we'd want those to be Standby NICs. To set the standby NICs, use this command:

	vmware-vim-cmd hostsvc/net/portgroup_set --nicorderpolicy-standby=<List of VMNICs> <vSwitch#> <Portgroup>

Run the command to set the active NICs first, then run the command to set the standby NICs. The following example will set vmnic0 as Active and vmnic1 as standby for the Service Console port group on vSwitch0:

	vmware-vim-cmd hostsvc/net/portgroup_set --nicorderpolicy-active=vmnic0 vSwitch0 "Service Console"  
	
	vmware-vim-cmd hostsvc/net/portgroup_set --nicorderpolicy-standby=vmnic1 vSwitch0 "Service Console"

I hope this is helpful to some readers.

**UPDATE:** A reader clarified in the comments that the parameters can actually be combined into a single command, like so:

	vmware-vim-cmd hostsvc/net/portgroup_set --nicorderpolicy-active=vmnic0 --nicorderpolicy-standby=vmnic1 vSwitch0 "Service Console"

I tested this syntax on VMware ESX 3.5 Update 3 and it does work as expected. Thanks, Timo!
