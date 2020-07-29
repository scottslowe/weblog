---
author: slowe
categories: Tutorial
comments: true
date: 2009-08-13T22:50:08Z
slug: getting-qlogic-gen2-cnas-working-with-esx-4
tags:
- Cisco
- FCoE
- Hardware
- Nexus
- Virtualization
- VMware
- vSphere
- Storage
title: Getting QLogic Gen2 CNAs Working with ESX 4
url: /2009/08/13/getting-qlogic-gen2-cnas-working-with-esx-4/
wordpress_id: 1525
---

If you've been following my updates on [Twitter](http://twitter.com/scott_lowe), you know that I'm in the process of installing and configuring a Nexus 5000 switch. The Nexus 5010 will provide 10Gb Ethernet and Fibre Channel over Ethernet (FCoE) to the servers and storage arrays in the lab, and I'll be using the equipment to help generate reference architectures and standard configurations for the [ePlus](http://www.eplus.com/) virtualization team.

One of our storage partners sent over some second-generation (Gen2) Qlogic QLE8142 dual-port converged network adapters (CNAs) for me to use. These adapters joined the Emulex LP21000 CNAs that I had already procured for the lab, and I'd use the Qlogic and Emulex CNAs to connect my HP DL385G2 servers to the Nexus 5010. I don't know if it's because these cards are cutting edge (they are pre-production/beta units, apparently), but if all Nexus and FCoE deployments are this difficult to get up and running then FCoE will have a bleak future indeed. I've managed to finally get them working and recognized, but it most certainly was not as straightforward as I had anticipated.

Here's what I've had to do to make them work (with thanks to Eric H for his help):

1. I flashed the HP DL385G2 to the latest firmware available from HP. I don't know if this is absolutely necessary (I'll find out when I do the next server), but I figured it couldn't hurt.

2. Next I had to obtain a special beta flash image of the firmware and BIOS for the CNA and flash it to the latest version. The first time I flashed it, the CNA reported errors during the POST on the next reboot. I repeated the process and the errors went away.

3. The in-box drivers for VMware ESX 4.0.0 don't support the Gen2 QLogic cards, so I had to obtain beta drivers to install into VMware ESX. I first tried `esxupdate` with the bundle ZIP file, using the command `esxupdate update --bundle=offline-bundle.zip --nosigcheck`. I couldn't omit the `--nosigcheck` parameter because `esxupdate` then failed with error 20 (VibSigMissing). That, unfortunately, worked only for the Ethernet drivers and not the HBA drivers. To make the HBA drivers install, I had to install the RPM directly and include the `--force` parameter because ESX thought the version that was already installed was newer than the beta driver I was trying to install. I did finally get both sets of drivers installed, though.

Once both sets of drivers were installed, the 10GbE NICs and the 10Gb HBAs were both recognized in vCenter Server, but the ESX console showed an error about missing signatures. You can see a [screenshot of the error here](/public/img/esx4-gen2cna-drivers.png).

I'm quite confident that this is just because this card is an early Gen2 card, and that the strangeness I've been seeing is not a reflection on QLogic or VMware. I am a bit disappointed that the CNA is not supported by ESX 4.0.0 out of the box; I guess that will probably be remedied in ESX 4.0 Update 1. (No, I don't have any special insider knowledge. I'm just guessing.)

Even so, I wanted to share this information with the readers so they could be prepared in case they are planning an FCoE deployment. Just make sure that you are prepared to update the BIOS and firmware of your server and potentially your CNAs, and be prepared to install custom drivers. Note that I'm going to try a fresh install of ESX 4 on this system and install the drivers during the installation process to see if that helps to correct the errors I've seen thus far. I'll post an update to this article when I have more information.

**UPDATE:** Flashing the HP DL385G2 BIOS isn't necessary; after posting this article, I tried the same procedure with a server using an older BIOS. The card worked fine. Also, I made a _major_ mistake in the original version of this article---the cards are actually QLogic cards, not Emulex cards. The Emulex LP21000 CNAs that I have are Gen1 CNAs and, aside from a strange cabling issue I'm chasing, appear to work just fine.
