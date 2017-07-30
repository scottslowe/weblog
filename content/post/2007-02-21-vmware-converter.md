---
author: slowe
categories: Review
comments: true
date: 2007-02-21T16:47:19Z
slug: vmware-converter
tags:
- P2V
- Virtualization
- VMware
title: VMware Converter
url: /2007/02/21/vmware-converter/
wordpress_id: 416
---

Over the last week or so, I've had the opportunity to use [VMware Converter](http://www.vmware.com/products/converter/) in two different settings to perform two different types of physical-to-virtual (P2V) conversions. In one case, we booted from the Converter bootable CD image to perform an offline conversion; in the second instance, we used Converter to stream an image while the source machine was still online. In addition, one of the conversions was to an [ESX Server](http://www.vmware.com/products/vi/esx/) farm, while the other was to [VMware Server](http://www.vmware.com/products/server/) for proof-of-concept and testing. Here are some observations on each of these conversions.

## Offline P2V to ESX Server

The first conversion I did was using VMware Converter's boot CD to move a workload on a physical server over to an ESX Server managed by [VirtualCenter](http://www.vmware.com/products/vi/vc/) (and a member of a DRS/HA cluster). The overall process was very straightforward---boot the server on the CD, walk through the "Import Machine" wizard, etc. The only odd thing we ran into was that Converter refused to log in to VirtualCenter. We tried short hostname, fully qualified hostname, and IP address, and still had zero luck getting Converter to connect to VC. Fortunately, connecting directly to one of the ESX servers using the root account worked without any problems whatsoever, and the overall conversion process took about 1 hour and 20 minutes to move the 12 to 13 gigabytes of data on the source server. As fully expected, the new VM it created worked just fine. (There were some DNS-related issues after starting the new virtual server, but I feel these were completely unrelated to Converter or the P2V process.)

I'm still not sure why Converter wouldn't connect to VC; the only thing that may be a contributing factor was that the VC server was running on a non-standard port. However, even when specifying the port number, Converter still wouldn't connect. and the error message that was displayed after a failed attempt kept referencing TCP port 902. I also found that the delays when attempting to connect were _horrendous_; three or four attempts to connect took up well over 35 or 40 minutes of time just waiting for Converter to come back and tell us it couldn't connect. I hope to be able to try another offline P2V in this same environment soon to see if we can isolate and address the issues we ran into this time around. If anyone else has run into this issue and has a workaround, please share it in the comments.

### What I Like:

* Able to import directly to VMFS on ESX Server farm, no need to use a helper VM or `vmkfstools`

* Installed VMware Tools automatically during the process

* Good network throughput (roughly 8-9GB per hour)

### What I Don't Like:

* Had a problem logging in to VirtualCenter, had to connect to back-end ESX server instead

* Seemed a bit slower to boot up than the older P2V Assistant (this is a _very_ subjective assessment)

## Online P2V to VMware Server

I also used VMware Converter to do an "online" P2V; that is, to make a virtual image of an existing physical machine while the physical machine was still up and running. While this process requires some logistical coordination for the cut-over, it also eliminates the majority of the downtime typically involved in a P2V conversion. The target ESX server wasn't yet up and running in this particular case (another story for another day, trust me), so we used a Windows host running VMware Server as our target.

Once again, the process was very simple and straightforward. Using the VMware Converter Manager on another host (the machine running VMware Server itself, actually), we selected a remote machine, installed the agent (which required a reboot, by the way), and then started the P2V process after the reboot. At first, the process was _very_ slow; it was estimating almost 6 hours to pull across the roughly 6GB of data on the remote PC. After a while, though, the pace increased significantly and the overall time to pull across all the data was only about an hour and twenty minutes. Still, when you compare this rate to the rate achieved in the offline conversion, you see that it is much slower. Of course, there were differences in the environment (different network topologies, different source and target systems, etc.), so this isn't necessarily an apples-to-apples comparison. It does, however, give you a rough idea of what you to expect.

There were a few rough edges, though nothing major. You apparently can't install VMware Tools as part of the conversion; this adds an extra step to the configuration of the VM after it is created. It also seemed to take a few minutes before Sysprep launched on the VM; in fact, I thought that Sysprep wasn't even going to be invoked at all. Neither of these issues are major issues, however.

### What I Like:

* Minimal downtime required on source physical machine (just a reboot, and sometimes not even that---apparently depends upon version of Windows)

### What I Don't Like:

* Slower network throughput than offline conversion

* Can't install VMware Tools during conversion

## VMware Server to ESX Server

This last test surprised me, primarily because I was under the impression that VMware Converter Starter Edition wouldn't support ESX Server as a target platform. In this particular instance, we hadn't bothered to get the Enterprise license installed because this was testing/proof of concept. Although Converter wouldn't let us go from physical machine to ESX Server, it would let us go from physical machine to VMware Server and then to ESX Server.

The downside, of course, is that this adds an extra step to the process. The upside is that this extra step only took about 13 minutes to move the data from VMware Server to ESX Server (_much_ less time than it took to get the data into VMware Server to start with). After taking an hour and a half or so to get the data onto VMware Server, another 15 minutes to get it into ESX Server is not, in my opinion, a big deal.

This time around we were again able to install the VMware Tools during the conversion and ask for Sysprep to be run to customize the guest (although I had to reboot the VM once before the mini-Setup appeared). After Sysprep ran and went through mini-Setup, I noticed that VMware Tools were being reported as "out of date"; a quick perusal showed that the VM was running the VMware Server build of VMware Tools, not the ESX Server build.  Hence, the "out of date" indication. Further, once you did launch an installation of VMware Tools from the ESX Server, the installer detected the previous installation and offered the typical "Remove/Repair/Modify" dialog box, rather than performing an upgrade of the tools. Again, I'm not sure why that was the case. It's an issue that's relatively easily resolved, but it would've been nice to know that the "Instal VMware Tools" checkbox wasn't really going to help at all.

### What I Like:

* Speed (didn't take long to go from VMware Server to ESX Server)

* No need for a helper VM or vmkfstools

* Can install VMware Tools during migration (see below)

### What I Don't Like:

* Apparently doesn't install ESX version of VMware Tools

All in all, VMware Converter is a very handy application. I plan to continue to perform some testing with Converter in more environments and more scenarios over the next few weeks, and I will post additional information at that time.

As always, feel free to share any tips, tricks, thoughts, suggestions, or corrections in the comments below.
