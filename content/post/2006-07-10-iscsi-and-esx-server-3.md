---
author: slowe
categories: Explanation
comments: false
date: 2006-07-10T09:11:00Z
slug: iscsi-and-esx-server-3
tags:
- ESX
- Interoperability
- iSCSI
- Linux
- Microsoft
- NAS
- NetApp
- RedHat
- Storage
- Virtualization
- VMware
- Windows
title: iSCSI and ESX Server 3
url: /2006/07/10/iscsi-and-esx-server-3/
wordpress_id: 295
---

A few weeks ago I wrote about trying to use [NetApp's ONTAP Simulator as a VM under ESX Server][1] so that I could do some testing with the new NAS and iSCSI functionality in [ESX Server 3.0](http://www.vmware.com/products/vi/esx/). I finally got that working, but later had to shut it down when I started working with ESX Server 3. As it turns out, the ProLiant 6400R servers I was using for my VMware lab (running ESX 2.5.x) were not supported for the final release of ESX 3 because the `cpqarray.o` driver was dropped from the final release, and the `cpqarray.o` driver is what supported the Smart Array 3200 and Smart Array 4250 RAID controllers in these two boxes. No ESX 3 on these boxes.

Thankfully, I was able to locate an unused ProLiant ML350 G4p on which I could run ESX Server 3. Instead of trying to use the ONTAP Simulator as a VM, then, I rebuilt one of the 6400R servers with Red Hat Linux 9.0 and installed the [NetApp](http://www.netapp.com/) ONTAP Simulator directly from there. After a fair amount of work, I finally had everything setup, and the simulated Filer was running with about 20GB of available storage to serve up via iSCSI, CIFS, NFS, or HTTP.

iSCSI was what I was really interested in, so the available storage from the ONTAP Simulator got carved into a couple of LUNS to be presented via iSCSI, along with the requisite initiator groups. Once everything looked to be in place on the Simulator, I moved to the configuration of ESX Server. Had I been lucky, I might have been trying to do this after [Mike Laverick](http://www.rtfm-ed.co.uk/) released his [Service Console Guide](http://www.rtfm-ed.co.uk/?p=261), but I was a few days too early, and used the VI Client GUI instead to configure the VM Kernel NIC and the software iSCSI client. The configuration looked correct, but there was no connectivity.

Not sure if the problem was ESX Server or the ONTAP Simulator, I mapped another LUN and created another initiator group and tested iSCSI connectivity from a [Windows Server 2003](http://www.microsoft.com/windowsserver2003/) VM using Microsoft's [iSCSI initiator](http://www.microsoft.com/downloads/info.aspx?na=22&p=1&SrcDisplayLang=en&SrcCategoryId=&SrcFamilyId=&u=%2fdownloads%2fdetails.aspx%3fFamilyID%3d12cb3c1a-15d6-4585-b385-befd1319f825%26DisplayLang%3den). That worked _perfectly_---not even the first problem. Clearly, the problem was not with the ONTAP Simulator, but with ESX Server instead.

Reviewing the configuration, I did find a couple of problems:

* The iSCSI node name I had specified in the igroup configuration on the Simulator was incorrect, so I fixed that. (Or thought I had; more on that in a moment.)

* The iSCSI security configuration was incorrect, so I fixed that as well.

ESX Server should see the storage now, but there was still no connectivity. Finally, I came across a blurb somewhere about the new firewall in ESX Server 3.0, and how it controlled not only inbound traffic, but also _outbound_ traffic. A quick trip to the service console and this command:

    esxcfg-firewall -e swISCSIClient

You would expect that using the VI Client to enable the software iSCSI client would also enable outbound iSCSI traffic support on the ESX firewall, but it didn't. As soon as I entered that command, the Simulator's console (where I was logged in) started showing iSCSI connection requests. This, in turn, revealed another problem---ESX Server insisted on using its own iSCSI node name instead of the node name I had assigned. That was easily and quickly corrected, and I was finally able to mount the iSCSI LUN and create a VMFS datastore.

Key points to remember:

* Apparently, the iSCSI node name you specify in configuring ESX Server will be ignored, so don't bother. Just use whatever ESX is already configured with.

* Be sure to _either_ configure iSCSI from the command line (where outbound iSCSI traffic is allowed through the firewall automatically) or go back and allow outbound iSCSI traffic through the ESX firewall.

Having iSCSI-based storage eliminates one potential block to testing and demonstrating VMotion to customers---shared storage---but now I need to work on getting Gigabit Ethernet into the test lab as well. Too bad there's not a software workaround for that, too...

**UPDATE:** I corrected the command-line above to reflect that the first "I" should be uppercase, not lowercase as was previously noted. Thanks to Chauncey for catching the error!

[1]: {{< relref "2006-06-27-netapp-ontap-simulator-and-esx-server.md" >}}
