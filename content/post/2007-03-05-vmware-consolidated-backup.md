---
author: slowe
categories: Explanation
comments: true
date: 2007-03-05T15:46:38Z
slug: vmware-consolidated-backup
tags:
- Storage
- VCB
- Virtualization
- VMware
title: VMware Consolidated Backup
url: /2007/03/05/vmware-consolidated-backup/
wordpress_id: 424
---

[VMware Consolidated Backup](http://www.vmware.com/products/vi/consolidated_backup.html) (VCB) is a new component of Virtual Infrastructure 3, and is designed to facilitate both full-VM and file-level backups of ESX-hosted virtual machines on a SAN. In its current release, it only supports Fibre Channel SANs, but support for iSCSI is supposedly coming in the next release. When used in conjunction with a third-party backup application (such as [Backup Exec](http://www.symantec.com/backupexec/index.jsp)) and the appropriate integration software, VCB can provide the ability to backup VMs across the SAN (instead of across the network) without the need to install backup agents on every VM. The speed of backups is pretty good, too.

We performed a number of backups with VCB during our tests:

* File-level backup of Windows-based guest with the guest OS running  
* File-level backup of Windows-based guest with the VM powered off  
* Full VM backup of Windows-based guest with the guest OS running

All of the preliminary VCB tests listed above were performed using `vcbMounter`, a command-line tool installed on the VCB proxy server. The command we used looked something like this (this has been line-wrapped for readability, but should be entered all on a single line):

```text
vcbmounter -h vcenter.example.com -u vcbservice 
-p <password> -a ipaddr:10.1.1.100 -r E:\VMmount\VM1 
-t file -m san
```

In the specific environment in which this testing was conducted, the hostname (of the VirtualCenter server, in this example) had to be changed from the default of 902, as the customer was using a non-default port number. This threw us for a minute, until we could determine exactly what port on which the server was listening.

This mounted the contents of this VM's virtual hard disks on the path `E:\VMmount\VM1\letters\C`, `E:\VMmount\VM1\letters\D`, etc. We could then, of course, manually launch a backup of the files, but instead we continued with our preliminary testing and chose to wait on the Backup Exec testing until we were comfortable with `vcbMounter`.

This command worked, but in order to back up the virtual machine while it was shut down, we had to change the command slightly (again, this is line-wrapped for readability):

```text
vcbmounter -h vcenter.example.com -u vcbservice 
-p <password> -a name:VM1 -r E:\VMmount\VM1 
-t file -m san
```

Here, the "name:VM1" parameter was the name of the VM that we wanted to back up _exactly as it appears in VirtualCenter, including case._ (We did try this command using the same name with in different case, but it failed.) We could also have used the BIOS UUID for the VM, which can be retrieved using this command:

```text
vcbvmname -h vcenter.example.com -u vcbservice 
-p <password> -s name:VM1
```

Again, the name has to match exactly what is listed in VirtualCenter. One of the parameters returned by this command is the BIOS UUID, which you can then use in a `vcbMounter` command like this (this would be entered on a single line, not wrapped for readability as it is here):

```text
vcbmounter -h vcenter.example.com -u vcbservice 
-p <password> -a uuid:<BIOS UUID> -r E:\VMmount\VM1 
-t file -m san
```

By using the VM name or the VM BIOS UUID, we were able to make `vcbMounter` work both when the VM was running as well as when the VM was shutdown. Using the VM's IP address or DNS name, on the other hand, only worked when the VM was up and running.

Once we felt comfortable with `vcbMounter` and `vcbVmName`, we begain testing of the Backup Exec Integration Module (BEIM), a set of freely downloadable scripts designed to provide some automation between VCB and Backup Exec. Although the syntax of Backup Exec's integration scripts was a bit odd (and I think the documentation was incorrect in spots), the scripts worked well. Using the supplied scripts, we were able to perform both file-level and full-VM backups of selected virtual machines without any manual intervention required (the scripts handled all the mounting/dismounting/etc.). Watch out for spaces in the VCB path or in the VM names, though; they'll cause the supplied scripts to fail. [This article](http://searchservervirtualization.techtarget.com/tip/0,289483,sid94_gci1233940,00.html) offers some extensions to the Backup Exec scripts that help address some of the shortcomings, including correcting the problem with spaces in the path or the VM name.

While VCB does have its shortcomings, it's still a very useful tool to have in your backup arsenal. Between VCB full-VM and file-level backups and agent-assisted backups with an agent in the guest OS, there are plenty of ways to protect your virtualized servers.
