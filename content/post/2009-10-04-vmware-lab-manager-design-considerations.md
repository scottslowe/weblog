---
author: adelp
categories: Education
comments: true
date: 2009-10-04T23:18:37Z
excerpt: I've done a number of VMware Lab Manager white boarding sessions, and I want
  to share a few of my design notes and the reason for each.
slug: vmware-lab-manager-design-considerations
tags:
- Virtualization
- VMware
- vSphere
- GuestPost
title: 'VMware Lab Manager Design Considerations '
url: /2009/10/04/vmware-lab-manager-design-considerations/
wordpress_id: 1678
---

_By Aaron Delp_

I have done a number of VMware Lab Manager white boarding sessions and I want to share a few of my design notes and the reason for each. Most of items come from my installation experience and the version 4 release notes. Here they are in no particular order.

* **You need an LDAP server to import groups** - Yes, you can set up user IDs in Lab Manager but you CAN NOT create groups. Groups must be imported into the LM Server from an LDAP server. This is critical if you intend to do any kind of restrictions around lease durations of configurations or storage pools.

* **You need fully qualified name resolution (with functioning reverse look-up) between all clients, ESX/vSphere servers, the LM Server, and the vCenter Server -** The clients need the ESX/vSphere servers because if this isn't in place, remote control of the virtual machines will not function (you get a black screen). You also need DNS entries for the LM server because if you implement LiveLink functionality, LiveLink is hardcoded to the LM server name. Lastly, you need the vCenter Server for behind the scenes communication of the LM environment.

* **Workflow and Disk Chains will be the key to success or failure of your project** - The VMware documentation does a great job of describing how to do things. But, the documentation falls on its face when it comes to describing WHY you should do things. The behind the scenes architecture must be planned out very specifically for Lab Manager to perform as you would expect. I will be covering Work Flow and Disk Chains in a future article.

* **Lab Manager version 4 Host Spanning requires an Enterprise Plus subscription because the VMware Distributed Switch technology is required** - In the previous version of Lab Manager, VMware HA, DRS, and VMotion were not supported if you set up a fenced (isolated by a NAT router) configuration. The configurations (a configuration is groups of VMs in Lab Manager) were pinned to an ESX server at time of creation and stayed with that server until destruction. LM version 4 gets around this by using the Distributed Switch to span hosts. Some people will want this feature but in my experience some will not want to pay the extra dollars just to get this one feature. Also, be aware that _the Cisco 1000V isn't supported with Lab Manager_.

* **You will need to monitor the number of configurations per server if you do not use Host Spanning** - Lab Manager deploys new configurations to the ESX/vSphere servers using round robin. A configuration is removed from a host when it is undeployed. It is possible that the workload in your cluster will become out of balance because certain machines "live" longer than others. Take this example; you have two vSphere servers and ten configurations with only one VM to keep it easy. The configurations will be deployed split in between the servers for a total of five per server. A user removes the configurations on the first server but leaves the configurations on the second server. Now another ten configurations are deployed. The new ten configurations will again be deployed five each. You know have one server with five and one server with ten. Over time your load will become unbalanced.

* **Lab Manager 4 can't use vSphere's thin provisioning for disks** - Lab Manager uses the concept of Linked Clones for copies of virtual machines. The first one in the chain is "thick", the rest of the machines are delta disks of the first one. This is a technology that is independent and different from both vSphere and VMware View.

* I haven't had a chance to test this one yet but it appears **VMware is now supporting and suggesting that you run the Lab Manager 4 server in a virtual machine**. Be careful with this one! Where are you going to install it? Are you going to install it on the same cluster and ESX servers you are managing? This will create a circular dependency that I just don't like. Same goes for vSphere. Have you ever tried to patch ESX servers using Update Manager when vCenter Server is on that host? I have by accident, it doesn't work! For my lab I have my vCenter and Lab Manager servers in a different cluster (Scott's vSphere cluster) than my LM vSphere servers. In this configuration the servers are virtual, but they are out of the way. I know with Lab Manager 3 you couldn't install LM server on a virtual machine if it detected it was hosted on the ESX server you are trying to manage. I'm not sure if version 4 still has this checking feature included at installation.

* **Make sure you back up your Crypto Key that Lab Manager generates during the installation** - See the Manual for more information. You will need it if you run into big issues and need to reinstall Lab Manager.

* **VMware Snapshots are not supported because Lab Manager has a Snapshot feature built-in** - Lab Manager allows the user to take one (and only one!) snapshot of a configuration.

* **Increase the Service Console on ESX/vSphere servers from 272MB to 800MB** - This will help with the overhead of Lab Manager on the servers.

* VMware Fault Tolerance (FT) and Distributed Power Management (DPM) are not supported with Lab Manager.

* Storage VMotion and VMware VCB are not supported with Lab Manager.

If you have any other suggestions or design considerations, please let me know!
