---
author: slowe
categories: Explanation
comments: false
date: 2006-06-29T16:32:23Z
slug: strange-esx-server-error
tags:
- ESX
- Virtualization
- VMware
title: Strange ESX Server Error
url: /2006/06/29/strange-esx-server-error/
wordpress_id: 284
---

I spent the day today working with [ESX Server 3.0](http://www.vmware.com/products/vi/esx/) and [VirtualCenter 2.0](http://www.vmware.com/products/vi/vc/), the next-generation products from [VMware](http://www.vmware.com/). After shutting down one of the older ESX 2.5.3 servers in the test lab and then booting it back up again, I ran into an error trying to start some of the VMs.

At first, I tried starting the VM (which, again, was hosted on an ESX Server running version 2.5.3 Upgrade Patch 1) from VirtualCenter 2.0. The error was something along the lines of "General System Error, VirtualCenter is Unable to Set the Power State of the Server" (or similar). Thinking perhaps that it was an incompatibility between the new version of VirtualCenter and the older version of ESX Server, I opened an SSH session to the Service Console and tried using vmware-cmd.

From the Service Console, I used this command:

    vmware-cmd /home/vmware/vsbgw01/vsbgw01.vmx start

I received an error there, too; this error was something along the lines of:

    There was an error connecting to the specified virtual machine: 
    Unexpected response from vmware-authd: 
    The process exited with an error:
    Cannot change mode of /var/run/vmware to 01777: 
    Operation not permitted
    Failed to initialize the VMX VMDB instance

A quick search on the Internet turned up this [VMTN community forum thread](http://www.vmware.com/community/message.jspa?messageID=388432), where I learned the problem was permissions on the `/var/run/vmware` directory. The thread has all the pertinent information to fix it, and after running the command mentioned in the thread all the VMs started up just fine---both from the Service Console command line as well as from VirtualCenter.

I'm not sure how the permissions got changed, but in the event you run into that error yourself, here's the fix.

**UPDATE:** I ran into this problem again, and to make finding the solution easier, I'm including the full details here instead of making readers follow the link. Basically, the error is the result of losing the "sticky bit" on the `/var/run/vmware` directory, and running the command `chmod 01777 /var/run/vmware` as root will fix the problem. The correct permissions for the directory should be "drwxrwxrwt" in the directory listing.
