---
author: slowe
categories: Information
comments: true
date: 2011-04-09T14:08:42Z
excerpt: This post is a follow up to a post from December 2009 discussing the use
  of Time Machine in Mac OS X 10.6, aka Snow Leopard, with an Iomega ix4-200d. Some
  people have suggested newer Iomega firmware changes the conclusions of that article,
  so I decided to see for myself.
slug: revisiting-snow-leopard-and-time-machine-on-an-iomega-ix4-200d
tags:
- Backup
- EMC
- macOS
- Storage
title: Revisiting Snow Leopard and Time Machine on an Iomega ix4-200d
url: /2011/04/09/revisiting-snow-leopard-and-time-machine-on-an-iomega-ix4-200d/
wordpress_id: 2265
---

In late 2009, I posted [a how-to on making Snow Leopard work with an Iomega ix4-200d for Time Machine backups][1]. I'll recommend you refer back to that article for full details, but the basic steps are as follows:

1. Use the `hdiutil` command to create the sparse disk image with the correct name (a concatenation of the computer's name and the MAC address for the Ethernet interface).

2. Create a special file inside the sparse disk image (the `com.apple.TimeMachine.MachineID.plist` file).

3. Put the sparse disk image on the TimeMachine share on the ix4-200d (if you didn't create it there).

4. Set up Time Machine as normal.

In the comments to the original article, a few people suggested that newer firmware revisions to the Iomega ix4-200d eliminated the need for this process. However, in setting up my wife's new 13" MacBook Pro, I found that this process **is still necessary**. Even though my Iomega ix4-200d is now running the latest available firmware (the 2.1.38.xxx revision), her MacBook Pro---running Mac OS X 10.6.7 with all latest updates---would not work with the Iomega until I manually created the sparse disk image and populated it with the `com.apple.TimeMachine.MachineID.plist` file. Once I followed those steps, the laptop immediately started backing up.

So, it would seem that even with the latest available firmware on the ix4-200d, it's still necessary to follow the steps I outlined in [my previous article][1] in order to make Time Machine work.

[1]: {{< relref "2009-12-09-snow-leopard-time-machine-and-iomega-ix4-200d.md" >}}
