---
author: slowe
categories: Information
comments: true
date: 2007-12-03T13:52:53Z
slug: oddity-with-windows-nfs-server
tags:
- Microsoft
- NFS
- Windows
title: Oddity with Windows NFS Server
url: /2007/12/03/oddity-with-windows-nfs-server/
wordpress_id: 586
---

I was reconfiguring NFS on the Windows Server 2003 R2-based file server providing both CIFS and NFS storage to the lab at the office when I ran into a strange issue.

I had started configuring some NFS exports, testing them to ensure that they worked as I expected. Along the way, I decided I wanted to restructure the directory hierarchy I was using, so I deleted the folders and rebuilt them from scratch. When I went to re-export one of the folders via NFS, a very non-descriptive and non-helpful dialog box popped up, indicating only that "the alias specified was already in use".

I immediately suspected the NFS services, but no amount of restarting the services or the whole server would fix the issue. The NFS server management console was not useful; it does not provide a list of current exports so that you can see where you may have already exported a path and used an alias.

After a few minutes of messing around, I recreated the original path. Upon immediately recreating the bottom-level folder, it immediately became an NFS export. What had happened was that I had created a folder, exported it, and then deleted the folder without removing the export. The export information stayed in the Registry (location below) even when the file system location was not present. In addition, the service did not log any information to the event logs indicating that a folder was missing or couldn't be found. The only error was the "specified alias is already in use" dialog box that appeared when attempting to export a different folder with the same name.

Upon digging around in the Registry, I found this location for NFS export data:

	HKEY_LOCAL_MACHINE\Software\Microsoft\Server for NFS\CurrentVersion\Exports

Under this key is a zero-based list---where the first export starts as 0--that contains the NFS export information. I deleted the key and now exporting the new folder with the original alias works as expected.

I suppose this behavior is expected; after all, the NFS server can only export a single path with any given name. I just expected more information to be available somewhere along the way, be it a warning in the Event Log that states the exported path is no longer available, or a message in the dialog box that indicates what path is currently exported with the desired alias. If the export was created months or even years ago, or created by a different administrator, it would be impossible to know where to look or where to go without digging into the Registry. If Microsoft wants to continue to warn users about directly modifying the Registry, then they'd probably better provide some non-Registry tools to manage this stuff.
