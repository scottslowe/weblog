---
author: slowe
categories: Explanation
comments: true
date: 2008-10-27T09:36:00Z
slug: quick-note-on-esx-and-esxi-storage-multipathing
tags:
- ESX
- ESXi
- NetApp
- SAN
- Storage
- Virtualization
- VMware
title: Quick Note on ESX and ESXi Storage Multipathing
url: /2008/10/27/quick-note-on-esx-and-esxi-storage-multipathing/
wordpress_id: 1001
---

Rich Brambley (of [VM /ETC](http://vmetc.com/)), Duncan Epping (of [Yellow Bricks](http://www.yellow-bricks.com/)), and I were having a brief discussion on Twitter late last week about storage multipathing. Rich initiated the discussion with [this update](http://twitter.com/rbrambley/status/973665521), which prompted me to respond and thus started the conversation. Rich and I continued the conversation via e-mail (Twitter isn't exactly the best medium for that kind of exchange), and prompted by his questions I did some digging. Here's what I came up with.

This [NetApp KB article](https://now.netapp.com/Knowledgebase/solutionarea.asp?id=kb44784) (NOW login required to view) is kind of what kick-started the entire thing. In that article, NetApp recommends that users set the preferred path for every LUN anytime an ESXi server is rebooted. This behavior seemed curious to me, so I inquired with some industry contacts to see where this recommendation may have originated.

One of my contacts responded that the `vicfg-mpath` command, part of the Remote CLI used to manage ESXi, is broken in that it lists the target's WWN _not_ the target's WWPN. Apparently this will be addressed in a future version of the `vicfg-mpath` tool. To compound the issue, setting preferred paths from VirtualCenter is apparently not persistent across reboots because the VML LUN name is not used. Hence, when working with ESXi, neither method of setting the preferred path---`vicfg-mpath` or VirtualCenter---can provide persistent settings. Hence, the recommendation in the NetApp KB article.

But what about VMware ESX? The issue about using VirtualCenter remains, but `esxcfg-mpath` allows users to use the VML LUN name during the command to ensure that paths are persistent across reboots. In fact, there's a note in the help screen of `esxcfg-mpath` (`esxcfg-mpath -h`) that mentions the fact that VMHBA names are not persistent across reboots, and users should use VML LUN names to be sure of consistency.

So there you have it---when using ESXi you'll want to set the preferred path for a LUN when you reboot, but if you using ESX you can avoid that trouble by using the VML LUN name in the `esxcfg-mpath` command. As clear as mud, right?
