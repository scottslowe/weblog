---
author: slowe
categories: Rant
comments: true
date: 2008-01-07T16:11:20Z
slug: underwhelmed-by-the-remote-cli
tags:
- CLI
- ESX
- ESXi
- Virtualization
- VMware
title: Underwhelmed by the Remote CLI
url: /2008/01/07/underwhelmed-by-the-remote-cli/
wordpress_id: 600
---

Along with the release of VMware Infrastructure 3 version 3.5 and ESX Server 3i version 3.5, VMware released the VMware Remote CLI. This was supposed to be, to my understanding, the new way to manage ESX Server 3i since there is no longer a Service Console with ESX Server 3i. Of course, you can still manage ESX Server 3i with VirtualCenter, but the Remote CLI was the only reasonable way to get a command line interface. In addition, the Remote CLI is the method whereby users can initiate a Storage VMotion operation.

Given the relative importance of the Remote CLI, I expected more. After visiting the download page for the Remote CLI and seeing, once again, only Windows and Linux versions, I downloaded the Remote CLI appliance. It seemed logical to me that I should be able to import the virtual appliance into my DRS/HA cluster and simply run it from there.

Unfortunately, things didn't work quite as I had planned. Despite repeated attempts, the import of the virtual appliance from the *.OVF file into VirtualCenter continually failed when attempting to map the networks for the virtual appliance. No amount of fiddling with the *.OVF file to put in network names that I was currently using would fix the problem. If anyone has a workaround for this particular issue, I'd love to hear it.

Seeing that the virtual appliance was apparently a dead end, I proceeded to install the Remote CLI on my VirtualCenter server. When the installation was complete, I found that I had no visible way of even knowing if the installation was successful. No new icons, no Start Menu items, no pop-up dialog box asking me if I'd like to run the Remote CLI...nothing. Only by opening a command prompt and navigating to the directory where the Remote CLI had been installed was I able to determine that something had actually happened. And what had happened? VMware had installed ActivePerl and a set of Perl scripts. That's all.

I did recall seeing [this warning from Bob Plankers](http://lonesysadmin.net/2007/12/11/vmware-rcli-windows-and-libxml2-errors/) about logging out and logging back in again, so I did that. It didn't really make any difference. I can run Remote CLI commands without having to specify the path, but I do have to specify the ".pl" extension on everything. And the scripts seem slow, although that may be attributable to the slightly older hardware I have in my lab.

Something about all this just seems really unpolished. Unfinished. Rushed. Pick whatever adjective suits you best here; I just know that this was _not_ what I had expected. Why call it the "Remote CLI" if its really just a collection of scripts that operate remotely? Wouldn't "ESX Server 3i Management Tools" be better? And yes, perhaps I am being a bit picky, but sometimes it's the little things that can make or break a customer's first impression. Would you want your customer's first impression of VMware's products to be the Remote CLI?
