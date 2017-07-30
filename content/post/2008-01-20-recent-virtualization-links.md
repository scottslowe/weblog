---
author: slowe
categories: Information
comments: true
date: 2008-01-20T21:42:09Z
slug: recent-virtualization-links
tags:
- ESX
- Networking
- Snapshots
- Virtualization
- VMware
- VMwareHA
title: Recent Virtualization Links
url: /2008/01/20/recent-virtualization-links/
wordpress_id: 611
---

Over the last few weeks, I've been collecting various virtualization-related links in NetNewsWire's Flagged Items collection, with the intention of blogging about them, bookmarking them, or both. With time a bit short recently---let's just say that life is really, really busy right now---I decided to just condense a bunch of them here with a brief commentary, where applicable, for each. Hopefully some of this information will prove useful to some readers here.

* [ESX Host Currently Has No Management Network Redundancy Error](http://www.planetmy.com/blog/esx-host-currently-has-no-management-network-redundancy-error/): This is new to ESX Server 3.5; VMware HA reports a warning when it detects that there is no redundancy for the Service Console. Clearly, this is an attempt to prevent situations where isolation response kicks in, and as the author points out can be mitigated by adding another NIC to the vSwitch where the Service Console port group is located. I have also found that creating a second Service Console port group on another vSwitch will also remove the warning. Duncan of Yellow Bricks also goes into more detail on [Service Console redundancy](http://www.yellow-bricks.com/2008/01/14/service-console-redundancy/) on his blog as well.

* [ESX "Configuring for HA" errors - What to do?](http://itknowledgeexchange.techtarget.com/virtualization-pro/esx-configuring-for-ha-errors-what-to-do/): VMware HA continues to be a sore spot, as Rick Vanover discusses here. One useful tidbit of information from this article is the suggestion to go directly to the VPX_EVENT table of the VirtualCenter database to look for troubleshooting information. Rick's right---VirtualCenter's error messages with regards to VMware HA are often totally useless.

* [How to Use the Remote Command-line Interface to Invoke Storage Vmotion in Windows Server or Desktop](http://vmwareworld.blogspot.com/2008/01/how-to-use-remote-command-line.html): Jack's off to a great start to his blog at VMware World with a lot of very relevant and very useful information. This article on using the RCLI to do Storage VMotion can come in handy at times, until you get the hang of it. On a related note, Duncan hits us up with some information on [useful add-ons for Storage VMotion](http://www.yellow-bricks.com/2008/01/04/storage-vmotion-add-ons/).

* [Virtual Machine High Availability](http://www.yellow-bricks.com/2008/01/03/todo-virtual-machine-high-availability/): Still listed as an "experimental" feature in VI3 version 3.5, if I recall correctly, Virtual Machine HA uses heartbeats from the VMware Tools inside a guest to try to determine if a guest has failed. Anyone out there doing more than just experimenting with this?

* [Delete all snapshots](http://www.yellow-bricks.com/2008/01/07/delete-all-snapshots/): For those end users that don't work with snapshots, this article is a _must read_.

* [VMotion Is Disabled After ESX Server Upgrade](http://www.vmwarewolf.com/vmotion-is-disabled-after-esx-server-upgrade/): This can be handy if you were wondering why VMotion suddenly stopped working after the upgrade to ESX Server 3.5.

* [Migration will cause the virtual machine's configuration to be modified](http://www.yellow-bricks.com/2008/01/02/migration-will-cause-the-virtual-machines-configuration-to-be-modified/): It's still not clear exactly _why_ VirtualCenter is making some changes to virtual machines during a live migration. Duncan's explanation about virtualized MMU and paravirtualization support in ESX Server 3.5 makes sense, but what about the commenter's issue with a migration from ESX Server 3.0.1 to ESX Server 3.0.2? That doesn't seem to make any sense, especially on identical hardware.

Anyone with additional information on any of these topics is invited to speak up in the comments.
