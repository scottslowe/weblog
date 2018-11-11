---
author: slowe
categories: Explanation
comments: true
date: 2009-01-05T12:33:11Z
slug: changing-vnic-port-group-assignment-in-vmware-with-powershell
tags:
- ESX
- Networking
- Virtualization
- VLAN
- VMware
title: Changing vNIC Port Group Assignment in VMware with PowerShell
url: /2009/01/05/changing-vnic-port-group-assignment-in-vmware-with-powershell/
wordpress_id: 1119
---

Experienced PowerShell users won't find this post very helpful, but less experienced PowerShell users---or even PowerShell newbies such as me---may find this handy. Today I had a need to change the port group assignment on the vNICs for a bunch of guest VMs in the lab. Rather than manually click through all these VMs just to change the port group, I decided to give PowerShell a try.

Thanks to [this post](http://professionalvmware.com/2008/12/18/1-day-left-the-most-awesome-powershell-one-liner-in-the-history-of-powershell-one-liners/) by Cody Bunch and [this Twitter response](http://twitter.com/halr9000/statuses/1097365098) by Hal Rottenberg, I cobbled together this PowerShell command:

```text
get-datacenter "Name" | get-vm | get-networkadapter | where-object { $_.networkname -like "OldPortGroup" } | set-networkadapter -networkname "NewPortGroup" -Confirm:$false
```

It worked like a champ! Obviously, you could limit the scope of this command by filtering the VMs that are returned with a wildcard pattern on the `Get-VM` command.

Thanks to Cody and Hal for their assistance!
