---
author: slowe
categories: Explanation
comments: true
date: 2007-12-05T23:04:10Z
slug: managing-lun-space-requirements-with-netapp-storage
tags:
- NetApp
- ONTAP
- SAN
- Snapshot
- Snapshots
- Storage
title: Managing LUN Space Requirements with NetApp Storage
url: /2007/12/05/managing-lun-space-requirements-with-netapp-storage/
wordpress_id: 589
---

If you've worked with Network Appliance storage before, you're probably already familiar with the idea of snap reserve (storage space set aside to accommodate for Snapshots) and fractional reserve (used with LUNs). I'm going to hold the in-depth discussion of why you need snap reserve and fractional reserve for a different day, but I did want to pass on these commands that were shared with me by a colleague of mine. These Data ONTAP commands, available with Data ONTAP 7.2 or later (some commands are available in Data ONTAP 7.1), will help you manage the space requirements for LUNs on a NetApp storage area network (SAN).

I'll try to explain the commands along the way, but I would recommend you review the documentation available from the NOW site for more complete information.

	vol options <volname> fractional_reserve 0

This command sets the fractional reserve to zero percent, down from the default of 100 percent. Note that fractional reserve only applies to LUNs, not to NAS storage presented via CIFS or NFS.

	snap autodelete <volname> trigger snap_reserve

This sets the trigger at which Data ONTAP will begin deleting Snapshots. In this case, Snapshots will start getting deleted when the snap reserve for the volume gets nearly full. The current size of the snap reserve can be viewed for a particular volume with the `snap reserve <volname>` command.

	snap autodelete <volname> defer_delete none

This command instructs Data ONTAP not to exhibit any preference in the types of Snapshots that are deleted. Options for this command include "user_created" (delete user-created Snapshot copies last) or "prefix" (Snapshot copies with a specified prefix string).

	snap autodelete <volname> target_free_space 10

With this setting in place, Snapshots will be deleted until there is 10% free space in the volume.

	snap autodelete <volname> on

Now that the Snapshot autodelete options have been configured, this command will actually turn the functionality on.

	vol options <volname> try_first snap_delete

When a FlexVol runs into an issue with space, this option tells Data ONTAP to first try to delete Snapshots in order to free up space. This command works in conjunction with the next command:

	vol autosize <volname> on

This enables Data ONTAP to automatically grow the size of a FlexVol if the need arises. This command works hand-in-hand with the previous command; Data ONTAP will first try to delete Snapshots to free up space, then grow the FlexVol according to the autosize configuration options. Between these two options---Snapshot autodelete and volume autogrow---you can reduce the fractional reserve from the default of 100 and still make sure that you don't run into problems taking Snapshots of your LUNs.

If you have a NOW login, you can get more information on Snapshot autodelete [here](http://now.netapp.com/NOW/knowledge/docs/ontap/rel723/html/ontap/onlinebk/2snap15.htm); more information on volume autogrow is available [here](http://now.netapp.com/NOW/knowledge/docs/ontap/rel723/html/ontap/mgmtsag/6vol10.htm). Be aware that SnapDrive may require different settings in order to accommodate its functionality, as it moves LUN management out of the storage system and onto the host. Finally, the values presented here are only examples; be sure to use values that are appropriate for your environment.

Credit for compiling this list goes to my colleague Chauncey Willard. Good work!
