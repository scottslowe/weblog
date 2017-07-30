---
author: slowe
categories: Rant
comments: false
date: 2006-01-10T19:04:18Z
slug: gsx-server-upgrade-woes
tags:
- Linux
- Networking
- Virtualization
- VMware
- Windows
title: GSX Server Upgrade Woes
url: /2006/01/10/gsx-server-upgrade-woes/
wordpress_id: 155
---

The last few days have been interesting ones for me. For those of you that didn't already know, I'm a network/systems engineer, and as such I'm involved in a variety of projects with a variety of customers. Just the other day, I had the opportunity to perform an upgrade of [VMware GSX Server](http://www.vmware.com/products/gsx/) on Windows Server 2003, from version 2.5 to version 3.2.1. Here are all the gory details.

Regular readers of this weblog know that I'm a huge fan of virtualization, and VMware's products---as the industry leaders in this technology---are particular favorites. I was excited, therefore, to get to participate in this upgrade. I reviewed the VMware documents in advance, noting the need to uninstall the previous version before installing the new version, and noting that virtual hardware would have to be upgraded before they would be able to take advantage of all the latest features. OK, seems fairly straightforward, so we shut down the virtual machines (VMs) and got started.

The uninstall of GSX Server 2.5 is reasonably painless, except for the Blue Screen of Death that assails us halfway through the installation. We still haven't determined exactly what caused the BSOD, but fortunately the host server booted back up again and showed no signs of further problems. Thinking that the GSX Server 2.x uninstall was complete given that the product was no longer listed in Add/Remove Programs, I launched the GSX Server 3 installer only to be greeted with a message stating that the previous version had to be uninstalled first.

Fortunately, another trip to Add/Remove Programs showed a GSX Server entry lingering there, and I was able to finally uninstall the application completely (requiring another reboot in the process). After the server came back up, I installed GSX Server 3.2.1 quickly and without incident.

Next, I made a backup copy of a small Linux VM running a 2.4.x kernel. This would be the first VM back up and running under the new version of GSX Server. When the backup was complete, I tried to upgrade the virtual hardware. The attempt failed; I learned I had to install the latest version of the VMware Tools in order to upgrade the virtual hardware. OK, fair enough, but these VMs didn't have the VMware Tools installed because they are text-only servers (they don't run the X Window System). For now, the customer and I agree to leave the existing Linux VM intact and just run it "as is."

I boot the server with "legacy" virtual hardware, and it boots perfectly and the hardware detection application launches, finding some changes in the "hardware" and wanting to reconfigure the hardware. A bit wary, I allow the application to update the virtual server's configuration, and I find that the process completes quickly and without any problems. Note that this reconfiguration included the network card, which had me worried that I'd have to recreate the network configuration. I did not---that's good news.

A second text-only Linux VM, also with a 2.4.x kernel, comes up as well and I am pleased to note that it behaves _exactly_ as the first one did. Repeatable results are good, in my opinion. Network connectivity to both VMs is fine; their network services are functioning perfectly despite the fact that GSX Server 3 sees them as "legacy VMs." Now on to a Windows-based VM.

I pick a non-essential Windows VM for testing. The initial boot is identical to the others. Once I get logged in, I install the new VMware Tools, which installs new versions of the video, mouse, network, audio, and SCSI drivers. Unfortunately, during the process of installing the updated drivers, the previous versions are completely removed. This _completely_ erases the network configuration of the Windows-based virtual server, which I must then recreate.

&lt;aside&gt;This is _very irritating_. VMware needs to work on this process. In this case, it wasn't a huge deal, but with a large number of VMs that would have to be reconfigured it could get ugly real quick. It can be done with Linux, why not with Windows? OK, that's probably a loaded question.&lt;/aside&gt;

Once the network settings are properly reconfigured, I shut the server down and upgraded the virtual hardware. This process took _far_ longer than I had anticipated it would, apparently needing to somehow reconfigure the virtual disk files. After a lengthy delay, the virtual hardware upgrade is complete and I boot the Windows VM again. As expected, the New Hardware Wizard launches once I get logged into the VM. Not as expected, however, the wizard can't find drivers for the virtual audio device that has been introduced as a result of the virtual hardware upgrade. I disable the device and continue; servers don't need audio anyway.

I still have a couple more virtual machines to boot as of this writing; however, these are pre-production systems (still in testing and validation) so there is no terrible rush just yet. So far, I'm both pleased and displeased with GSX Server 3.2.1 compared to previous versions. I have another server to upgrade soon; that upgrade will be particularly interesting because it hosts some Linux VMs with a 2.6.x kernel. These 2.6.x kernels seem to be a bit problematic under GSX Server 2.5.

As the upgrades continue, I'll post additional information. In the meantime, if you are a VMware expert and want to post some comments telling me how I could have handled this situation better, please do! I'm always seeking information to help me improve.
