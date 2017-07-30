---
author: slowe
categories: Musing
comments: true
date: 2007-09-06T16:33:29Z
slug: invalid-configuration-error-creating-new-vms
tags:
- ESX
- Storage
- Virtualization
- VMware
title: Invalid Configuration Error Creating New VMs
url: /2007/09/06/invalid-configuration-error-creating-new-vms/
wordpress_id: 525
---

Earlier today, I was in [VirtualCenter](http://www.vmware.com/products/vi/vc/) and needed to create a new virtual machine (VM). As this VM was going to be for purely scratch purposes, I wanted to use some local storage on one of the ESX servers. There was no need for VMotion, or DRS, or HA; it was a non-critical workload. So I right-clicked on the cluster, selected "New Virtual Machine...", and started walking through the wizard to create a new VM. When prompted for storage, I selected a local datastore. Upon the end of the wizard, I clicked Finish, and was promptly greeted with an error message stating:

	Invalid configuration for device '4'

Hmm...that's not a very helpful error message. Searching the web turned up [this VMTN discussion forums thread](http://www.vmware.com/community/message.jspa?messageID=688454), where apparently others have been running into this same issue. The problem appears to be linked to attempting to create a new VM within a DRS cluster, but selecting storage that is not available to all DRS cluster members (i.e., local storage or SAN storage that has not been presented to all DRS cluster members). If you select storage that all DRS members can see, then the error goes away.

Returning to VirtualCenter, I confirmed that this was indeed the case. Oddly enough, though, the error occurs whenever I select local storage on two of the [ESX Server](http://www.vmware.com/products/vi/esx/) hosts, but the error does not occur if I select local storage on the third host in the DRS cluster. All three hosts are running ESX Server 3.0.2 and all three are configured identically (as far as I can tell), but ESX02 works fine while ESX04 and ESX05 cause the error described above.

Even more strangely, right-clicking on a specific host within the DRS cluster and selecting "New Virtual Machine..." causes the error as well. I suppose I could see where [VMware](http://www.vmware.com/) would say that right-clicking on the DRS cluster itself would generate an error, because you're creating a new VM _on the cluster,_ not on a specific host. But if I right-click on a host, that generally means I want a new VM on that host. Yes, I know that DRS kicks in to optimize the placement of VMs onto hosts during creation as well as during operation, and this is probably what's kicking up the error, but I don't recall seeing this error when running ESX Server 3.0.1 and VirtualCenter 2.0.1 (I'm now running ESX Server 3.0.2 and VC 2.0.2).

Is this a bug? Any readers seeing this behavior, and do you have any workarounds or additional information beyond what is being presented in the relevant forums thread? And why does it only seem to affect some DRS cluster members, but not others?
