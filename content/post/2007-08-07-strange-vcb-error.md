---
author: slowe
categories: Explanation
comments: true
date: 2007-08-07T12:43:21Z
slug: strange-vcb-error
tags:
- Backup
- ESX
- SAN
- Virtualization
- VMware
title: Strange VCB Error
url: /2007/08/07/strange-vcb-error/
wordpress_id: 502
---

While in the process of verifying the operation of [VMware Consolidated Backup](http://www.vmware.com/products/vi/consolidated_backup.html) (version 1.0.3) today using the command-line `vcbMounter.exe` utility, I kept receiving an error from vcbMounter and the full VM backups would fail. Nothing seemed obvious at first, so I added the "-L 6" parameter to the command line, which was something like this:

	vcbmounter -h vcserver.example.com -u username -p password  
	-r e:\mnt -a ipaddr:10.1.1.107 -t fullvm -L 6

Nothing terribly complicated there, just a simple full VM backup of the VM whose IP address is 10.1.1.107. (For those of you that aren't familiar with the `vcbMounter.exe` command-line syntax, it looks worse than it actually is. Trust me.) Upon running this command with the increased logging, I kept getting these errors:

	[2007-08-07 12:13:47.418 'App' 2144 warning] Could not
	obtain inquiry page 128 for device on path 0, target 4, lun 0  
	
	[2007-08-07 12:13:47.418 'App' 2144 warning] Sending SCSI inquiry 
	failed: Unknown error. (No proper error code was returned.)  
	
	[2007-08-07 12:13:47.418 'App' 2144 warning] Could not 
	obtain inquiry page 128 for device on path 0, target 5, lun 0  
	
	[2007-08-07 12:13:47.418 'App' 2144 warning] Sending SCSI inquiry
	failed: Unknown error. (No proper error code was returned.)`

The odd thing was, target ID 4 and target ID 5 were _local SCSI targets_, not anything SAN-related. In fact, they were the system (C:) and data (D:) drives that had been created when the server was built and [Windows Server 2003](http://www.microsoft.com/windowsserver/default.mspx) was installed.

Google turned up nothing obvious, so I decided to try running the command directly against an ESX server. The modified command now looked like this:

	vcbmounter -h esxserver.example.com -u root -p password  
	-r e:\mnt -a ipaddr:10.1.1.107 -t fullvm -L 6`

The operation still failed, but now I had a critical piece of missing information:

	[2007-08-07 12:29:43.798 'BlockList' 2052 error] Your VirtualCenter or the ESX server hosting the virtual machine you are dealing with needs to be upgraded to work with this version of VCB. (VCB attempted to invoke the method "acquireLeaseExt" on a remote object of type "vim.host.DiskManager", but this method is unknown to this object type.)

Aha! A quick review of the environment showed that the ESX host this particular VM was hosted on was indeed running version 3.0.1. With a quick VMotion to a nearby host running ESX Server 3.0.2 and a repeat of the command (changed to target the new host, obviously), and the backup operation worked. I moved the guest back to the original host again, and the operation failed again. This pattern held true regardless of whether the `vcbMounter.exe` command targeted the VirtualCenter server (which was running version 2.0.2) or the ESX Server.  Anytime the VM was hosted on the ESX server running 3.0.1, the command failed.

&lt;aside&gt;Now why didn't the error message just say that the first time, instead of complaining about local SCSI disks?&lt;/aside&gt;

A quick review of the [VCB 1.0.3 release notes](http://www.vmware.com/support/vi3/doc/releasenotes_vcb103.html) turns up this fairly brief blurb:

>VMware Consolidated Backup (VCB) 1.0.3 is compatible with VirtualCenter 2.0.2 and ESX Server 3.0.2 (or newer) only. This release is not supported if used with older version of ESX Server and VirtualCenter.

OK, fair enough---I should have more closely read the release notes before getting too deep into the testing. But what this means is that customers won't be able to start using VCB 1.0.3 until all their hosts have been upgraded to ESX Server 3.0.2 and VirtualCenter has been upgraded to 2.0.2. I don't know at this time if VCB 1.0.2 will work against both the newer and older versions; if not, that will put organizations in a situations where they may end up with two different sets of VCB proxy servers: one set to support the hosts running ESX Server 3.0.1, and another set to support hosts running ESX Server 3.0.2. And that doesn't even take VirtualCenter into consideration!

Anyone out there testing VCB 1.0.2 against the newer releases of ESX and VC? This will help tell us if we can leverage the existing VCB infrastructure until after all the hosts have been upgraded, and then upgrade VCB, or if a parallel VCB infrastructure will have to be established to support the newer of ESX and VC.
