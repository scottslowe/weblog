---
author: slowe
categories: Tutorial
comments: true
date: 2009-05-21T13:49:17Z
slug: vmware-vsphere-vds-vmkernel-ports-and-jumbo-frames
tags:
- CLI
- ESX
- ESXi
- iSCSI
- Networking
- NFS
- Storage
- Virtualization
- VMware
- vSphere
title: VMware vSphere vDS, VMkernel Ports, and Jumbo Frames
url: /2009/05/21/vmware-vsphere-vds-vmkernel-ports-and-jumbo-frames/
wordpress_id: 1371
---

In April 2008, I wrote an article on [how to use jumbo frames with VMware ESX and IP-based storage][1] (NFS or iSCSI). It's been a pretty popular post, ranking right up there with the ever-popular article on [VMware ESX, NIC teaming, and VLAN trunks][2].

Since I started working with VMware vSphere (now [officially available][1] as of 5/21/2009), I have been evaluating how to replicate the same sort of setup using ESX/ESXi 4.0. For the most part, the configuration of VMkernel ports to use jumbo frames on ESX/ESXi 4.0 is much the same as with previous versions of ESX and ESXi, with one significant exception: the vNetwork Distributed Switch (vDS, what I'll call a dvSwitch). After a fair amount of testing, I'm pleased to present some instructions on how to configure VMkernel ports for jumbo frames on a dvSwitch.

## How I Tested

The lab configuration for this testing was pretty straightforward:

* For the physical server hardware, I used a group of HP ProLiant DL385 G2 servers with dual-core AMD Opteron processors and a quad-port PCIe Intel Gigabit Ethernet NIC.

* All the HP ProLiant DL385 G2 servers were running the GA builds of ESX 4.0, managed by a separate physical server running the GA build of vCenter Server.

* The ESX servers participated in a DRS/HA cluster and a single dvSwitch. The dvSwitch was configured for 4 uplinks. All other settings on the dvSwitch were left at the defaults.

* For the physical switch infrastructure, I used a Cisco Catalyst 3560G running Cisco IOS version 12.2(25)SEB4.

* For the storage system, I used an older NetApp FAS940. The FAS940 was running Data ONTAP 7.2.4.

Keep in mind that these procedures or commands may be different in your environment, so plan accordingly.

## Physical Network Configuration

Refer back to my first [article on jumbo frames][1] to review the Cisco IOS commands for configuring the physical switch to support jumbo frames. Once the physical switch is ready to support jumbo frames, you can proceed with configuring the virtual environment.

## Virtual Network Configuration

The virtual network configuration consists of several steps. First, you must configure the dvSwitch to support jumbo frames by increasing the MTU. Second, you must create a distributed virtual port group (dvPort group) on the dvSwitch. Finally, you must create the VMkernel ports with the correct MTU. Each of these steps is explained in more detail below.

### Setting the MTU on the dvSwitch

Setting the MTU on the dvSwitch is pretty straightforward:

1. In the vSphere Client, navigate to the Networking inventory view (select View > Inventory > Networking from the menu).

2. Right-click on the dvSwitch and select Edit Settings.

3. From the Properties tab, select Advanced.

4. Set the MTU to 9000.

5. Click OK.

That's it! Now, if only the rest of the process was this easy...

By the way, this same area is also where you can enable Cisco Discovery Protocol support for the dvSwitch, as I pointed out in [this recent article][3].

### Creating the dvPort Group

Like setting the MTU on the dvSwitch, this process is pretty straightforward and easily accomplished using the vSphere Client:

1. In the vSphere Client, navigate to the Networking inventory view (select View > Inventory > Networking from the menu).

2. Right-click on the dvSwitch and select New Port Group.

3. Set the name of the new dvPort group.

4. Set the number of ports for the new dvPort group.

5. In the vast majority of instances, you'll want to set VLAN Type to VLAN and then set the VLAN ID accordingly. (This is the same as setting the VLAN ID for a port group on a vSwitch.)

6. Click Next.

7. Click Finish.

See? I told you it was pretty straightforward. Now on to the final step which, unfortunately, won't be quite so straightforward or easy.

### Creating a VMkernel Port With Jumbo Frames

Now things get a bit more interesting. As of the GA code, the vSphere Client UI still does not expose an MTU setting for VMkernel ports, so we are still relegated to using the `esxcfg-vswitch` command (or the vicfg-vswitch command in the vSphere Management Assistant---or vMA---if you are using ESXi). The wrinkle comes in the fact that we want to create a VMkernel port attached to a dvPort ID, which is a bit more complicated than simply creating a VMkernel port attached to a local vSwitch.

**Disclaimer:** There may be an easier way than the process I describe here. If there is, please feel free to post it in the comments or shoot me an e-mail.

First, you'll need to prepare yourself. Open the vSphere Client and navigate to the Hosts and Clusters inventory view. At the same time, open an SSH session to one of the hosts you'll be configuring, and use `su -` to assume root privileges. (You're not logging in remotely as root, are you?) If you are using ESXi, then obviously you'd want to open a session to your vMA and be prepared to run the commands there. I'll assume you're working with ESX.

This is a two-step process. You'll need to repeat this process for each VMkernel port that you want to create with jumbo frame support.

Here are the steps to create a jumbo frames-enabled VMkernel port:

1. Select the host and and go the Configuration tab.

2. Select Networking and change the view to Distributed Virtual Switch.

3. Click the Manage Virtual Adapters link.

4. In the Manage Virtual Adapters dialog box, click the Add link.

5. Select New Virtual Adapter, then click Next.

6. Select VMkernel, then click Next.

7. Select the appropriate port group, then click Next.

8. Provide the appropriate IP addressing information and click Next when you are finished.

9. Click Finish. This returns you to the Manage Virtual Adapters dialog box.

From this point on you'll go the rest of the way from the command line. However, leave the Manage Virtual Adapters dialog box open and the vSphere Client running.

To finish the process from the command line:

1. Type `esxcfg-vswitch -l` (that's a lowercase L) to show the current virtual switching configuration.

	At the bottom of the listing you will see the dvPort IDs listed. Make a note of the dvPort ID for the VMkernel port you just created using the vSphere Client. It will be a larger number, like 266 or 139.

2. Delete the VMkernel port you just created with the command `esxcfg-vmknic -d -s <dvSwitch Name> -v <dvPort ID>`.

3. Recreate the VMkernel port and attach it to the very same dvPort ID with the command `esxcfg-vmknic -a -i <IP addr> -n <Mask> -m 9000 -s <dvSwitch Name> -v <dvPort ID>`.

4. Use the `esxcfg-vswitch` command again to verify that a new VMkernel port has been created and attached to the same dvPort ID as the original VMkernel port.

At this point, you can go back into the vSphere Client and enable the VMkernel port for VMotion or FT logging. I've tested jumbo frames using VMotion and everything is fine; I haven't tested FT logging with jumbo frames as I don't have FT-compatible CPUs. (Anyone care to donate some?)

As I mentioned in [yesterday's Twitter post](http://twitter.com/scott_lowe/status/1859891868), I haven't conducted any objective performance tests yet, so don't ask. I can say that NFS _feels_ faster with jumbo frames than without, but that's purely subjective.

Let me know if you have any questions or if anyone finds a faster or easier way to accomplish this task.

**UPDATE:** I've updated the comments to delete and recreate the VMkernel port per the comments below.

[1]: {{< relref "2008-04-22-esx-server-ip-storage-and-jumbo-frames.md" >}}
[2]: {{< relref "2006-12-04-esx-server-nic-teaming-and-vlan-trunking.md" >}}
[3]: {{< relref "2009-03-13-next-gen-stuff-enabling-cdp-in-esxesxi.md" >}}
