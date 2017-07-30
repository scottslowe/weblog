---
author: slowe
categories: Explanation
comments: true
date: 2006-12-06T17:02:09Z
slug: vm-portability-again
tags:
- ESX
- Macintosh
- Virtualization
- VMware
title: VM Portability Again
url: /2006/12/06/vm-portability-again/
wordpress_id: 381
---

The idea was to take a VM running [Windows Server 2003 R2](http://www.microsoft.com/windowsserver/default.mspx) off one of the ESX servers and port it over to VMware Fusion on my [MacBook Pro](http://www.apple.com/macbookpro/). If that worked (and I fully expected that it would), then I'd try reversing the process. In particular, I'd love to move that same VM I moved last week over to the VI3 server farm from my MBP (it's running the NetApp Data ONTAP Simulator and I could _really_ use it running on the servers). This attempt would be a precursor to that attempt.

First, the VMDK had to be exported out of the VMFS datastore into 2GB sparse format (the format used by VMware products other than ESX Server). In my case, that meant exporting a 4GB VMDK out of an iSCSI VMFS datastore to the local filesystem.

The command for that is:

    vmkfstools -i /path/to/src/vmdk /path/to/final/vmdk -d 2gbsparse

Of course, this line needs to be typed all on a single line, but you already know that. (Long-time ESX users will note that the `vmkfstools -i` listed above is the replacement for the now-deprecated `vmkfstools -e` command that was used in ESX 2.x and earlier.)

Once the data was available outside the VMFS datastore, I just need to get the data down to my MBP. [Cyberduck](http://cyberduck.ch/), an open source SFTP client for [Mac OS X](http://www.apple.com/macosx/), filled that need quite handily, and a while later I had the 3.0GB of data on my laptop.

VMware Fusion opened the .VMX file without any major complaints and promptly booted the VM, notifying me that it needed to create a new UUID, that the CMOS was invalid (I think this is a VMware Fusion bug, actually, since I get this for all new VMs created with Fusion), and that it couldn't load the networking. Ah, yes, I had forgotten that VMware Fusion only supports NAT networking at this stage of development.

I removed these two lines from the .VMX file:

    ethernet0.networkName = "Lab"
    ethernet0.addressType = "vpx"

And added this line:

    ethernet0.connectionType = "nat"

Upon rebooting the VM, no more warning messages. The VM booted up and everything seemed to work just fine. (There were some OS-specific issues, as this was a domain controller and now could no longer reach the domain, but those were to be expected in this kind of situation.)

Now the trick is to go the other way...to take a VM I have on VMware Fusion and push it up to ESX Server. I'll post more information here when I've had a chance to try that.
