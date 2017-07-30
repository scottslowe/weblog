---
author: slowe
categories: Tutorial
comments: true
date: 2007-05-11T12:38:01Z
slug: how-to-provision-vms-using-netapp-flexclones
tags:
- NetApp
- Storage
- Virtualization
- VMware
title: How to Provision VMs Using NetApp FlexClones
url: /2007/05/11/how-to-provision-vms-using-netapp-flexclones/
wordpress_id: 452
---

When properly implemented and configured, [VMware Virtual Infrastructure](http://www.vmware.com/products/vi/) can make provisioning new servers a task that takes only minutes. In fact, in my own lab (running equipment that is, admittedly, several years old and woefully underpowered), I can provision new servers running [Windows Server 2003 R2](http://www.microsoft.com/windowsserver/default.mspx) in less than 10 minutes. That's pretty impressive.

As impressive as those numbers may be (and I'm sure there are readers out there with even more impressive numbers), if we leverage some vendor-specific storage functionality we can achieve some really impressive times. For example, leveraging NetApp [FlexClones](http://www.netapp.com/products/enterprise-software/storage-system-software/provisioning-volume-management/flexclone.html) could allow us to provision new VMs in seconds. Let's take a quick look at how that's done.

In this article, I'm going to discuss how to use FlexClones for provisioning new VMs in a VMware VI3 environment. This is not an exhaustive treatise on the subject, but rather an introduction to the process and some of the configuration that needs to take place in your environment. (**_Disclaimer: Use this stuff at your own risk._**)

## Configuring ESX Server

First, we need to change the configuration of [ESX Server](http://www.vmware.com/products/vi/esx/) to enable it to see the FlexClones on the SAN. The change we need to make is to enable _resignaturing_; that is, to enable ESX to recognize an existing VMFS datastore even if it is presented on a different LUN ID than the LUN ID it had when it was created. When a VMFS datastore is created, ESX (or VirtualCenter) places a signature in the datastore that contains the LUN ID (among other information). If this datastore is then presented back out with a LUN ID that doesn't match the LUN ID in the signature, then it won't be recognized by ESX Server. Since we'll be using FlexClones to make identical copies of VMFS datastores (including their signatures) and then present them out as new LUNs (with different LUN IDs than the original), we need to enable resignaturing in order for ESX Server to see the new LUNs.

There are two ways to enable resignaturing:

* From the command line, type `esxcfg-advcfg -s 1 /LVM/EnableResignature` (you must be root)

* From VirtualCenter, select the ESX Server, go to the Configuration tab, select Advanced Setings, choose LVM from the list on the left, and then change the value of LVM.EnableResignature to 1

Once this change is set, ESX will recognize LUNs in FlexClones as "snap-XXXXXXXX-name". You can easily rename them once they have been added to VirtualCenter.

Please note that this process can introduce some oddities in your storage discovery/creation process. Make sure that you have the LUN properly recognized and configured for access by all applicable hosts before you start placing VMs on the LUN.

## Creating/Preparing VMs for Cloning

One advantage that VirtualCenter's cloning has over this technique is that the process of preparing a VM for cloning is all automated---VirtualCenter handles all that behind the scenes, launching SysPrep for Windows guests or using open source software for other guests. All an administator has to do is just make sure that SysPrep is installed on VirtualCenter properly.

In this process, the guest OS preparation has to be done manually, and the placement of VMs onto the VMFS datastores has to be considered. Since we will be making exact copies of the VMFS datastores, all VMs on that datastore will also be copied. If you are sure that one of the cloned VMs will never be started up from the cloned VMFS, then you can leave it alone, but any guest OS that will be started up in the cloned datastore will need to be prepared first. Again, for Windows guests, this means running SysPrep to generate new SIDs and reseal the operating system to factory defaults.

Let's say you wanted to be able to quickly provision servers running Windows Server 2003 using FlexClones. You'd need to first create a new VM and the accompanying VMDK files, selecting to put that onto a VMFS that is either a) empty and will contain only this VM; or b) contains VMs that _will not_ ever be powered on after they are cloned. You'd then need to install Windows Server 2003 on that VM, install VMware Tools (not required but very recommended), install any applicable patches or third-party software packages, and finally run SysPrep to prepare it for cloning. After all those steps have been done, you can create the FlexClone.

## Creating FlexClones on the Storage System

Please note that there is a tremendous amount of additional information pertaining to the use of Snapshots in VMware environments that I have not covered here. I highly recommend [TR 3428](http://www.netapp.com/library/tr/3428.pdf) from [NetApp](http://www.netapp.com/), which covers this information in detail, including best practices for volume configuration, Snapshot reserve, fractional reserve, etc.

Now, having said all that, and assuming that you've followed some of these guidelines, here's how we go about creating FlexClones on the storage system. (This assumes you've built a VM and prepared it for cloning as described in the previous section.)

While logged into the storage system with appropriate permissions, first take a snapshot of the FlexVol containing the LUN that has the VMFS datastore you want cloned. You can call this Snapshot something like "base\_clone\_snapshot" or similar, but be sure to use a name that makes sense to you and helps you understand the purpose of this snapshot. The command to do this would be:  

    snap create fvol_master clone_base_snapshot

This creates a Snapshot of the FlexVol "fvol_master" named "clone_base_snapshot".

Next, create a FlexClone based on the Snapshot you just created:  

    vol clone create fvol_clone1 -b fvol_master clone_base_snapshot

This creates a new FlexVol named "fvol_clone1", which is based on the Snapshot named "clone_base_snapshot" in the FlexVol "fvol_master".

Because this clone is an exact copy of the original flexible volume, including LUNs and LUN maps, Data ONTAP will spit out some messages about LUNs being taken offline and such. To fix this, unmap the LUN(s) in the new FlexClone and remap them with different LUN IDs:

    lun unmap /vol/fvol_clone1/lun_name igroupname
    lun map /vol/fvol_clone1/lun_name igroupname 3

Obviously, substitute the appropriate LUN ID for the "3" in the above command line. This remaps the LUN to the specified igroup with a new LUN ID and, assuming you've enable resignaturing, makes the LUN (which is a VMFS datastore) visible to ESX Server and VirtualCenter.

Next, unless you want Snapshots of the FlexClone, you'll need to disable scheduled Snapshots on the FlexClone using the `snap sched` command:  

    snap sched fvol_clone1 0

This disables scheduled Snapshots, but manual Snapshots are still allowed. (To disable all Snapshots, you'd need to set the "no_snap" volume option.)

At this point, you now have the original VMFS datastore and any virtual machines contained therein (contained in the LUN on the original FlexVol), as well as an exact copy of that VMFS datastore (contained in the LUN on the FlexClone).

## Registering the VMs

The VMs (comprised of the VMX, VMXF, NVRAM, and all VMDK files) were cloned along with the LUN and the FlexVol, but VMware doesn't know they are there.
In order for the VMs to be usable, we must first register them. To run the commands needed to register the VMs, you must log into one of the ESX servers as root. (Logging in via SSH as a normal user and then using `su` to escalate to root is fine, as is logging in as root at the console.)

Once logged in, you'll use the `vmware-cmd` utility to register the VMs. Let's assume that you called the FlexClone "san-lun-clone1" in VirtualCenter, and that a VM called "win1" exists on that VMFS datastore. The command to use would look something like this:

    vmware-cmd -s register /vmfs/volumes/san-lun-clone1/win1/win1.vmx

For each VM on the datastore that needs to be recognized by ESX (and has been properly prepared in advance, as noted above), repeat this process. With a little work, it should be fairly easy to write a script that finds all the *.vmx files on a datastore and registers them. (Anyone care to take up that challenge?)

At this point, you now have the following:

* The original SAN LUN, with all the VMs stored there

* A cloned SAN LUN, with all the same data as the original but occupying far less space than a traditional copy)

* VMs registered and ready for use from both SAN LUNs

Having already enabled resignaturing, created and prepared the VMs, and taken the base snapshot, you could now easily create additional clones by simply creating the FlexClone and registering the VMs. If you were to have a script that automated that process (perhaps using SSH shared keys or RSH to access the NetApp storage system from ESX), that entire process could be fairly easily automated. I'll leave that automation as an exercise for enterprising readers.

As a matter of best practice, please note that leaving resignaturing enabled (i.e., leaving the LVM.EnableResignature setting to 1) may lead to problems if LUNs are inadvertently re-signed. For long-term operation, I would advise users to disable resignaturing once cloned LUNs have been re-signed and are visible in the VI Client.

In future articles, we'll take a closer look at the question of "Should I use FlexClones?" instead of "How do I use FlexClones?".
