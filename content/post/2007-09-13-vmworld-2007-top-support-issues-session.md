---
author: slowe
categories: Liveblog
comments: true
date: 2007-09-13T22:02:57Z
slug: vmworld-2007-top-support-issues-session
tags:
- ESX
- Networking
- Virtualization
- VMware
- VMworld2007
title: VMworld 2007 Top Support Issues Session
url: /2007/09/13/vmworld-2007-top-support-issues-session/
wordpress_id: 544
---

My last session of Day 3, and of the entire conference, was a super-session on the top support issues and how to resolve them. For someone who wasn't already familiar with some of the Service Console command-line utilities (such as `esxcfg-vswitch`, `esxcfg-vmknic`, `esxcfg-vswif`, etc.), this was a great session. For someone already pretty comfortable with these tools, the session was less helpful.

The session centered around six or seven top support issues:

1. Unable to connect to the Service Console
2. NICs in a bond are not in the same broadcast domain
3. Expanding a VMDK when there is an existing snapshot
4. Corrupt snapshot (.VMSD) file
5. Corrupt snapshot
6. Adding extents to VMFS volumes
7. Recovering a VMFS partition

Possible causes of problem #1 include deleting the vSwitch that houses the vswif. To fix the problem, you can probably just switch out NICs (and use `esxcfg-nics` to unassign and reassign NICs to the appropriate vSwitch), adjust VLAN properties (using the `esxcfg-vswitch` command to modify the port group), or recreate the vswif interface (using the `esxcfg-vswif` command). In some cases, it may be necessary to completely recreate the networking configuration. The process for completely rebuilding the networking configuration looks like this:

1. Use `esxcfg-vswitch` to delete all vSwitches
2. Create a new vSwitch using `esxcfg-vswitch`
3. Create a new port group for the Service Console (again, using `esxcfg-vswitch`)
4. Link a physical NIC to the vSwitch
5. Create a vswif interface (using `esxcfg-vswif`) and configure it with the correct IP address, subnet mask, and default gateway

Many Service Console issues relate to changing the Service Console configuration. It's recommended to create a "backup" Service Console connection before modifying the primary connection, in case something doesn't work as expected.

Problem #2 I wasn't so clear on. I know what a broadcast domain is, but my experience with the "network hints" that are displayed in VirtualCenter and with `esxcfg-info` have been less than accurate. I guess the key take-away here is to make sure that NIC members in a bond are on the same VLAN or subnet, and that the network hints might help you determine if they are not.

Problem #3, expanding a .VMDK with an existing snapshot, is a bit more interesting. In this case, it's necessary to parse the delta disk (look for the line starting "RW"), take that value, and replace the value in the base disk's matching "RW" line with the value taken from the delta disk. It's then necessary to commit the snapshot using `vmware-cmd`.

With problem #4, the .VMSD file that tracks snapshots and snapshot relationships has become corrupt. Fortunately, it's reasonably easy to correct: delete the .VMSD, create another snapshot (which then recreates the .VMSD), and then commit all snapshots. As part of this process, you lose the ability to selectively rollback to a specific snapshot, but there is no data loss.

Problem #5 is similar, but the actual snapshot---the delta disk---is now the one that is corrupt. This typically occurs when the VMFS partition gets full (remember that a snapshot involves the use of a delta or "differencing" disk that stores all the changes to the base disk, and this can fill up a SAN LUN over time). In this case, there will be data loss. To fix the problem, we move the last delta file (or delete it) and edit the .VMX file to point back to the last intact snapshot. All changes since that last intact snapshot will be lost. It's recommended that all snapshots should then be committed using `vmware-cmd`.

With regards to VMFS volumes and adding extents, it's very important to perform a rescan (using `esxcfg-rescan` or VirtualCenter) on all ESX hosts after adding an extent. This is because when an extent is added its added on that host only, and if you then add an extent to that same VMFS partition from a different host, the VMFS partition will get corrupted.

Finally, the presenter went into some detail on recovering lost VMFS partitions. If it's only the partition information that's missing, that can be recovered using `fdisk`. If the partition was formatted....well, that's a much different story. Backups are the best response there.

We wrapped up the session with some support "best practices":

* Backups!
* Run `vm-support` (or gather diagnostics logs via VirtualCenter VI Client) periodically
* Implement change control in your environment
* Record the date and time when something happens, as this will help when trying to correlate log data

And with that, the session ended, and so did VMworld 2007. I was both thankful it was over, and yet disappointed that it had to end so soon.
