---
author: slowe
categories: Musing
comments: true
date: 2008-09-15T13:15:04Z
slug: follow-up-on-the-vdc-os-announcement
tags:
- Cisco
- ESX
- ESXi
- Networking
- Storage
- VCB
- Virtualization
- VMware
- VMworld2008
title: Follow Up on the VDC-OS Announcement
url: /2008/09/15/follow-up-on-the-vdc-os-announcement/
wordpress_id: 900
---

Now that I've gotten through the [first part of discussing VMware's VDC-OS announcement][1]---making sure that the message and vision is a bit clearer---I can focus on some of the specifics contained within the announcement. In other words, I can talk about new features. (That should [make Duncan happier](http://www.yellow-bricks.com/2008/09/15/vmwares-first-announcements/).)

  * VMware Fault Tolerance (FT), formerly "Continuous Availability," (demoed at VMworld 2007, described in [my liveblog here][2]) provides real-time VM mirroring between two different hosts. If a host fails, the mirrored VM on the secondary host picks up automatically with no disruption to the users. Marathon Technologies is working on similar functionality for Citrix XenServer, and both companies are expected to deliver next year. If VMware wants a leg up on the competition, they need to deliver this first.

  * The Distributed vSwitch enables administrators to define network settings at the cluster level, instead of on a host-by-host basis. This is **huge** for larger shops. Define your port group once, and you're done.

  * As has been [pointed out elsewhere](http://rationalsecurity.typepad.com/blog/2008/09/vmworld-2008-introducing-ciscos-virtual-switch-for-vmware-esx.html), Cisco is announcing the first third-party vSwitch for VMware ESX. Alessandro [points out rumors](http://www.virtualization.info/2008/09/what-to-expect-at-vmworld-esx-40-beta.html) that it will run NX-OS and will be a distributed vSwitch. In any case, it will certainly bring hard-core Cisco shops closer to embracing VMware as it will give them end-to-end control over the networking environment, all the way down to the virtual port level within any given VMware ESX host.

  * vStorage Thin Provisioning will help on storage demands. I suspect this is just the use of thin provisioned VMDKs, but if anyone has any other information to share I'd love to hear it.

  * Similarly, vStorage Linked Clones just brings to VMware ESX a feature that VMware Workstation and VMware Lab Manager have had for a while.

  * VMware will finally enter the backup market with vCenter Data Recovery, which (to my understanding) will leverage VCB to provide a backup solution for the virtual infrastructure.

That's quite an impressive list of features slated to be included in VDC-OS. What I'm also interested in seeing, though, is how the underlying components of VDC-OS---VMware ESX and VirtualCenter/vCenter---are going to change. I've seen rumors of 64-bit VMkernel and Console OS, and Duncan himself points out linked vCenters (which is quite cool, might I add). I suppose that will be part of the "Tech Preview" sessions that are going on later this week.

OK, that's it for now; need to run off for another session. Stay tuned here and on Twitter for more information!

[1]: {{< relref "2008-09-15-vmwares-virtual-datacenter-os.md" >}}
[2]: {{< relref "2007-09-13-vmworld-2007-day-3-keynote-liveblog.md" >}}
