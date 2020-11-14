---
author: slowe
categories: Tutorial
comments: true
date: 2010-07-17T20:54:16Z
slug: enabling-raid-1-on-a-mac-mini-server
tags:
- Apple
- macOS
- Storage
title: Enabling RAID 1 on a Mac Mini Server
url: /2010/07/17/enabling-raid-1-on-a-mac-mini-server/
wordpress_id: 1997
---

Today I bought a new Mac Mini running Mac OS X Server to replace an aging home-built Linux server that supports my home network. You might recall that in early 2009 I wrote an article about how I had worked to provide [some Ubuntu-Mac integration][1] on the home network. Although the integration has worked well since that time, many of the functions that the Ubuntu server was providing have since been taken over by an Iomega ix4-200d NAS box. It's the Iomega that now handles all my Time Machine backups and CIFS/AFP file sharing. In addition, I've been looking for a way to create a "master" iTunes library for the entire house, and the Linux server with Firefly Media Server, while powerful, just wasn't doing what I needed it to do. So I figured I'd replace it with an Intel-based Mac Mini running Mac OS X Server. The network services the Linux server is now providing would either be replaced by the Mac Mini or by VMs running on the Mac Mini vis [VMware Fusion](http://www.vmware.com/products/fusion/).

After I bought the Mac mini and brought it home, I was dismayed to find that Apple hadn't enabled RAID 1 on the dual 500GB hard drives in the system. Unfortunately, enabling RAID 1 mirroring on the hard drives wasn't as simple as using Disk Utility. I did find a way, and in the interest of helping others, here are the steps that I followed. As I type this, my Mac Mini server is downstairs rebuilding it's RAID 1 mirror set.

Here are the steps to follow to enable RAID 1 on the drives in your Mac Mini Server:

1. First, you'll need to boot from some source other than one of the internal hard drives. I chose to use the Mac OS X Remote Install application, found in the Utilities folder. This allows your Mac Mini server, which doesn't have an optical drive, to boot from the optical drive on another Mac on your network (that's assuming, of course, you have another Mac on your network). The procedure for booting remotely using the Remote Install application are already [well documented](http://support.apple.com/kb/HT2129), so I won't bother to include them again here.

2. Once you've booted remotely, open a Terminal window and run `diskutil list` to list the disks and volumes in your system. In all the online guides to this process, the "Server HD" volume was always listed as `disk0s2`; on my system, it was listed as `disk1s2`. Just be sure to note the device names so that you can use them later.

3. To convert the existing "Server HD" volume to a RAID 1 mirror, use the command `diskutil appleRAID enable mirror <device name for "Server HD">`. Mac OS X will convert the existing "Server HD" volume into a RAID 1 mirror with only one member. From what I can tell, this process is instantaneous and takes effect immediately.

4. Use the `diskutil list` command again to get the device name for the new RAID set. On my system, it was referenced as `disk9`, but this will vary from system to system.

5. Next, add the second hard disk as a member to the RAID 1 set using the command `diskutil appleRAID add member <device name for second hard disk> <device name for RAID set>`. For example, on my system the second hard disk was actually `disk0s2` and the RAID set was `disk9`, so the command looked like `diskutil appleRAID add member disk0s2 disk9`.

And that should do it! Mac OS X will add the second drive to the RAID 1 set and begin the rebuilding process. If you do this right after unboxing your new Mac Mini, it will minimize the amount of time required.

I'm still waiting on my Mac Mini to finish the rebuilding process, so I'll update this post if I discover something unusual or find that some additional steps are necessary to make everything work.

[1]: {{< relref "2009-01-02-ubuntu-and-mac-os-x-integration.md" >}}
