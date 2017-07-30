---
author: slowe
categories: Liveblog
comments: true
date: 2010-02-09T14:25:28Z
slug: partner-exchange-2010-session-techbc0320
tags:
- Microsoft
- Snapshot
- Virtualization
- VMware
- Windows
title: Partner Exchange 2010 Session TECHBC0320
url: /2010/02/09/partner-exchange-2010-session-techbc0320/
wordpress_id: 1822
---

This is a liveblog for VMware Partner Exchange session TECHBC0320, "How VMware Leverages Microsoft Volume Shadow Services for Virtual Machine Snapshots". The presenter is Paul Vasquez with VMware; he works within the Technical Alliances Organization at VMware with a focus on backups.

The session starts out with an overview of VMware snapshots followed by a quick overview of Microsoft Volume Shadow Copy Services.

Vasquez is careful to distinguish VMware snapshots from array-based snapshots, which is good since that seems to confuse a number of people. VMware snapshots can include the state of memory (optional), settings, and disk. Snapshots are taken at the VM level, and up to 32 snapshots can be taken. Over 20 snapshots can cause performance concerns and, in Vasquez's words, "can cause undesirable results".

In general, a snapshot will include all disks although there are ways to exclude disks from a snapshot.

Operations involving VMware snapshots include taking a snapshot (self-explanatory), reverting to a snapshot (reverts the VM to the snapshot state, the delta file remains until the snapshot is deleted), and deleting a snapshot (delta file is removed, VM continues running in the current state).

Some use cases for snapshots include: rollback capability for testing patches or updates; rollback for failed software installation; protection against unwanted results of OS reconfigurations or testing; backups (for creating consistent copies of a VM); and replication.

The delta file grows as-needed; over time, the delta file will grow larger and larger. Vasquez cautions attendees to be sure to plan datastore sizes to account for snapshots for VMs and the delta file growth caused by the changes to those VMs.

A good question was raised about read I/Os and the impact of snapshots. (Sorry, couldn't catch everything here.)

The presentation now moves on to a discussion of VSS. One component of VSS is the requestor; the requestor makes a request from a provider, and the writer provides information on how to provide information to a requestor. Providers are included with Windows and are responsible for intercepting I/O requests to create and represent volume shadow copies on the file system. There are also 3rd party providers. In this context of this discussion (VSS integration with VMware snapshots), VMware Tools is the requestor.

There is a wide range of applications that provide VSS support, including Exchange, SQL, SharePoint, Active Directory, BITS, DHCP, and WINS. The `vssadmin list providers` command will show all the providers. (Note that you won't see the VMware Tools when you run this command; it is dynamically loaded only at snapshot time and then unloaded.)

The `vssadmin list writers` command will show a list of writers.

The general flow of operation with VSS runs like this:

1. Requestor makes a shadow copy.

2. The writer is told to freeze all I/O.

3. The provider creates a shadow copy.

4. The writer is told to "thaw," or resume, I/O to the application.

5. The requestor now has access to the shadow copy.

The writer can support multiple enumerations, or different ways of coordinating the creation of the shadow copy. Exchange, for example, supports Full (backs up databases, logs, and checkpoints; truncates logs), Copy (backs up databases, logs, and checkpoints; does not truncate logs), Incremental (backs up and truncates logs), Differential (backs up logs but does not truncate). Of these, VMware uses the Copy enumeration when requesting shadow copies. Supposedly, the reason this is the case is to prevent interfering with backup applications that aren't aware that logs were truncated. In addition, when VMware calls VSS, _all_ writers are engaged, so it's not possible to selectively choose which VSS writers should be engaged (can't engage VSS for Exchange but not SQL within the same VM, for example).

In the future, VMware Tools will offer granular control over which VSS enumeration is used. Granular control over which VSS writers can be engaged is also planned.

Vasquez now moves into a discussion of how VMware snapshots and VSS integrate together. When a VMware snapshot is taken, this is when VSS integration comes into play. Obviously, for VSS integration the VM must be powered on (the guest OS must be running in order for VSS to be operational).

Some form of quiescing is always used when a snapshot is taken (unless the VM is powered off). The VMware Sync driver provides a crash-consistent copy of the VM but doesn't interact with applications. This option is available in vSphere 4.0 and can be used when no VSS support from the application is available. Obviously, there is VSS support (hence this session), and there are pre- and post-quiesce scripts that can be used to create homebrew solutions as well. Both VSS and the Sync driver can be enabled using VMware Tools.

VSS support is enabled in VMware ESX 3.5 Update 2 or higher.

Going back to the VSS flow earlier, an additional step is present before the writer resumes I/O to take the VMware snapshot. After the VMware snapshot is taken, the shadow copy created by the provider is discarded because it is no longer needed. Once again, Vasquez reminds attendees that the VMware Tools Requestor only supports the copy enumeration.

An attendee asked if any plans were in place to do quiescing at the VMFS layer (supposedly to assist with hardware-based snapshots); Vasquez responds that some form of VMFS quiescing would be helpful, but there are challenges with that arrangement that make it currently very difficult to actually achieve.

(Vasquez also commented on the end-of-life policy for the ESX Service Console, but I'll hold on mentioning what was said until I verify the confidentiality of the statement.)

Some additional things to remember:

* VMware Tools build must be 110268 or higher.

* VMware Tools must be running and VSS must be functioning properly.

* VSS Service must be set to Manual or Automatic.

* ESX 3.5 Update 2 is required for VSS support.

* Be sure VSS support is installed with VMware Tools.

* Try not to keep VMware snapshots around for a long time. Manage snapshots carefully.

* Sync driver can be used as a failback in the event VSS support fails.

* VSS snapshot has a 10 second timeout. Rare cases could cause a failure of getting the VSS shadow copy.

Most of the information contained in this presentation are found in the current vSphere documents and in Microsoft's VSS documentation. (I'll update this post with URLs when possible.)

And that's it for the session.
