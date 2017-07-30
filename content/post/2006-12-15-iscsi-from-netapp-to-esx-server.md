---
author: slowe
categories: Tutorial
comments: true
date: 2006-12-15T10:47:31Z
slug: iscsi-from-netapp-to-esx-server
tags:
- ESX
- Interoperability
- iSCSI
- NetApp
- ONTAP
- Storage
- Virtualization
- VMware
title: iSCSI from NetApp to ESX Server
url: /2006/12/15/iscsi-from-netapp-to-esx-server/
wordpress_id: 384
---

[Network Appliance](http://www.netapp.com/) storage systems are becoming more and more visible in [VMware](http://www.vmware.com/) deployments due to their iSCSI support and the synergies that are gained when combining VMware's functionality with the functionality of Network Appliance's Data ONTAP operating system. I'll touch on those synergies in a future article, but in the meantime here's how to configure a NetApp storage system to present iSCSI storage to one or more servers running [ESX Server](http://www.vmware.com/products/vi/esx/).

## Configuring the NetApp Storage System

Configuring the NetApp storage system is really pretty simple. (OK, so maybe it's only simple if you know what you are doing and have done it before.) It basically boils down to four steps:

1. Create the flexible volumes (FlexVols) that will store the LUNs for ESX Server.

2. Create the LUNs that ESX will use to establish a VMFS datastore.

3. Create the initiator group(s) (igroups) that will enable connectivity from the ESX server(s) to the NetApp storage system.

4. Map the LUNs to the appropriate igroup. This enables visibility of the LUNs to connected systems based on igroup.

Once these four steps are done, we are ready to move on to configuring ESX Server to utilize the NetApp-provided storage.

### Create the Flexible Volume(s)

As needed, you'll need to create flexible volumes (FlexVols) in which to store the LUNs. The command would look something like this:

	vol create fv_serverluns -s volume <aggregate name> 100g

This creates a 100GB FlexVol named "fv_serverluns" with a volume space guarantee (meaning that the 100GB is preallocated to the volume and unavailable for use by other volumes in the same aggregate).

Since the volume will serve as the container for the LUN(s), you'll need to create the appropriate FlexVols before creating the LUNs. Remember that the beautiful thing about FlexVols is that they can be _dynamically resized._

### Create the LUNs

Now we'll create the LUNs that will be exposed to the servers for their use. Use the following command to create a LUN:

	lun create -s 10g -t vmware <full path to LUN>

For full path to LUN, specify the name of the containing volume and the name of the LUN you'd like to create. For example, to create a LUN called "citrix\_lun" on our volume named "fv\_serverluns", the path would be:

	/vol/fv_serverluns/citrix_lun

Repeat this command for each LUN that needs to be created. Be mindful of the fractional reserve and snapshot reserve when allocating space in a volume to LUNs.

### Create the Initiator Group (igroup)

In order to make the LUNs visible to connected hosts, we must create an initiator group (igroup). In the iSCSI world, the igroup will contain the iSCSI node names of the initiators that will be connecting to the storage system.

To create an iSCSI igroup, use the following commands:

	igroup create -i -t vmware <igroup name>  
	igroup add <igroup name> <iSCSI node name of ESX host>

For each ESX host that will connect to these LUNs, use this command (or set default security):

	iscsi security add -i <iSCSI node name> -s CHAP -p <password> -n <username>

Once the igroup has been created and security set, then you can map the LUNs to the igroup to make them visible to the hosts.

### Map the LUNs

Mapping the LUNs merely makes a connection between the LUN and an associated igroup, which is (in turn) associated with a group of iSCSI initiators.

To map the LUNs, use the following commands:

	lun map <full path to LUN> <igroup name> <LUN ID>

For example, if you had a LUN named lun\_server1 in the flexible volume fv\_serverluns, then the full path to the LUN would be something like `/vol/fv_serverluns/lun_server1`. To connect that LUN to an igroup name "iscsi", you would use this command:

	lun map /vol/fv_serverluns/lun_server1 iscsi 0

That would map the LUN as LUN ID 0 to that initiator group. Of course, you must use a unique LUN ID for each LUN that you map to an initiator group. That is, LUN IDs must be unique within an initiator group, but not between initiator groups.

That's it. The rest of the work is up to ESX Server.

## Configuring the ESX Server(s)

Configuring the ESX Server(s) to see and use the new iSCSI storage being presented by the NetApp storage system can be broken down into 3 steps:

1. Enabling the software iSCSI initiator.

2. Configuring the iSCSI initiator for security and discovery and then discovering new LUNs.

3. Creating VMFS datastores on the new LUNs.

These three steps assume that you've already created a VMKernel port group for iSCSI and a matching Service Console port group on the same subnet. If you haven't already done that, go ahead and do that now.

Let's take a quick look at these steps in a bit more detail.

### Enabling the Software iSCSI Initiator

VMware ESX Server comes with a software iSCSI initiator that can be enabled and configured. To enable this software initiator, you can either use VirtualCenter or a command-line interface.

In VirtualCenter, enabling the software iSCSI initiator involves selecting a host from the inventory, going to the Configuration tab, selecting Storage Adapters, choosing the software iSCSI initiator (vmhba40), clicking on Properties, and then enabling software iSCSI. (Note: The dialog box for configuring software iSCSI doesn't dynamically update to show changes. You have to close the dialog box and re-open it to see your changes take effect.)

From the command-line (perhaps via an SSH session to the ESX server), you can simply type:

	esxcfg-swiscsi -e

That will enable software iSCSI. And while we're here at the command line, let's also enable outbound iSCSI traffic through the ESX firewall with this command:

	esxcfg-firewall -e swISCSIClient

Now we're ready to move on to configuring the initiator.

### Configuring the iSCSI Initiator

Once software iSCSI has been enabled, you'll need to configure it for security (authentication) and discovery. I haven't found a way to do this via the command-line (yet), so all this takes place in VirtualCenter (through the VI Client).

* From the Configuration tab of a host running ESX Server in VirtualCenter, click on the "Storage Adapters" link, then click on the software iSCSI initiator (vmhba40) and select Properties.

* On the Authentication tab, configure CHAP authentication and specify the same username and password you entered on the NetApp storage system earlier (when you were configuring the igroup).

* On the Discovery tab, add the IP address of the NetApp storage system as a dynamic discovery target. You can close the dialog box after this change is completed.

* Back in the VI Client, click to Rescan vmhba40 and discover new storage devices and new VMFS partitions. When the rescan is complete, you should see the new LUNs that were mapped from the NetApp storage system show up.

If the LUNs don't show up, then go back and troubleshoot the configuration---check IP addresses, verify connectivity, re-type usernames and passwords, look for typos in initiator node names or igroups, etc. Forgetting to enable the iSCSI traffic through the ESX firewall is another common mistake, so verify that with `esxcfg-firewall -q`.

### Creating New VMFS Datastores

Assuming your LUNs showed up correctly, then adding a new VMFS datastore is as simple as flipping over to the Storage section of the Configuration tab in VirtualCenter and clicking "Add Storage...". A nice wizard will walk you through selecting the LUN and creating a new VMFS datastore.

One quick note: you may find it most helpful to configure only one host first, then create all your VMFS datastores from that one host. Later, as you configure the additional hosts, the rescan will automatically find both the new LUNs and the VMFS datastores that reside on those LUNs, minimizing the number of times you have to perform a rescan.

That's it. As I mentioned earlier, in a future article I plan to touch on some of the advantages of using a NetApp storage system with VMware, as well as provide some technical details on how to take advantage of some NetApp-specific functionality with VMware.
