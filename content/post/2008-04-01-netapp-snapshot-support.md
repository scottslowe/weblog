---
author: slowe
categories: Information
comments: true
date: 2008-04-01T21:49:59Z
slug: netapp-snapshot-support
tags:
- Microsoft
- NetApp
- Snapshots
- Storage
- Windows
title: NetApp Snapshot Support
url: /2008/04/01/netapp-snapshot-support/
wordpress_id: 674
---

For readers that didn't already know this, Snapshots on NetApp-hosted CIFS shares are automatically and transparently recognized by Windows versions that support the "Previous Versions" tab. I believe this functionality is native on Windows Server 2003 and an add-on for Windows XP, but I'm not 100% certain. Either way, this means that users can easily recover files from NetApp Snapshots from within the Windows GUI.

Here's a screenshot of the Previous Versions support on an out-of-the-box server running Windows Server 2003 R2:

![NetApp Snapshots as Previous Versions](/public/img/netapp-ss-prev-vers.jpg)

As you can see in the screenshot, the Previous Versions tab automatically shows the NetApp Snapshots. No configuration is required; this is after a vanilla installation of Windows Server 2003 R2 and all applicable updates from Windows Update.

To NetApp veterans, this is nothing new, but I thought some new NetApp users out there might find information about this functionality and integration useful.
