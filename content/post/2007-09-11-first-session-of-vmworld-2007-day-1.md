---
author: slowe
categories: Liveblog
comments: true
date: 2007-09-11T19:59:27Z
slug: first-session-of-vmworld-2007-day-1
tags:
- Microsoft
- Virtualization
- VMware
- Windows
- VMworld2007
title: First Session of VMworld 2007, Day 1
url: /2007/09/11/first-session-of-vmworld-2007-day-1/
wordpress_id: 534
---

The morning of Day 1 of VMworld 2007 was filled with meetings, training sessions, and a self-inflicted mandatory visit to the Solutions Exchange (more on that tomorrow, at which time you'll understand why I can't say anything now and why I referred to it as a "self-inflicted mandatory visit"). In any case, it's now 2PM on Day 1 here in San Francisco and I just finished standing in line for about 15 minutes to get into a session on protecting VirtualCenter. Given that VirtualCenter is not a clusterable product that can be protected using Microsoft Cluster Server, anything that I can do to help harden VirtualCenter or increase VirtualCenter's fault tolerance is a definite plus.

The session is just now getting started, and so far the agenda looks like exactly what I was hoping to see---local availability with VMware HA and MSCS, cross-site availability, DR scenarios, etc. Excellent! It looks like the extremely long line for the session will be worth it.

The failure of VirtualCenter will impact VMotion (not available), VMware DRS (not available), but will not impact individual VMs (continue to run), individual ESX hosts (continue to run), or---I was not aware of this---VMware HA. It turns out that VMware HA, once configured, relies only upon ESX Server for functionality. In a situation where VirtualCenter is down, VMware HA will continue to operate. That's handy.

It is recommended to put the VirtualCenter server, the VMware license server, and the Web Access server on the same server. This session is going to focus on providing protection for these three components. The VC database can be protected using "industry standard" methods (read: MSCS or equivalent clustering functionality).

If the license server fails, there is a 14 day grace period, so this means that the failure of the license server component is not a time-sensitive failure that must be addressed immediately.

The VirtualCenter Server is almost stateless; there are only a few files stored locally that must be managed: the SSL certificate (used to access the VirtualCenter database), the VirtualCenter configuration file, and any upgrade files (these are used to modify the behavior of the VirtualCenter agents on the ESX servers). There is also the license file, of course, which is used by the license server itself. Everything else is stored in the VirtualCenter database.

Unfortunately, the second presenter in this session has an incredibly heavy foreign accent, and he's presenting the information  on providing fault tolerance for VirtualCenter. I hope I can understand what he says.

There are a couple of different ways to use VMware HA to provide protection for VirtualCenter. Of course, this only works for virtual instances of VirtualCenter, which begs the question of whether VirtualCenter should be run on a physical server or a virtual server. In any case, using VMware HA to protect VirtualCenter:

* Requires only a single OS instance
* May be subject to network and storage constraints (like what?)
* Will protect you from host hardware failure, but will not protect you against application failure or OS failure

Using MSCS to protect VirtualCenter, on the other hand:

* Requires VirtualCenter 2.0.2 patch 2 or later
* Works for physical or virtual instances (although using MSCS for virtual instances introduces limitations on VMware DRS, VMotion, and  VMware HA because of MSCS configuration requirements)
* Requires two OS instances (and requires some file copying and file synchronization, but there should be some easy ways to handle that)
* Needs some additional configuration in order to work correctly

The presenters provided a couple of different ways to use VMware HA, including out of band (using two VC instances in two different VMware HA clusters, each managing the other) and in-band (VC instance on top of a VMware HA cluster managing that same cluster).

When using MSCS to protect VirtualCenter, VMware recommends the use of Majority Node Set (MNS) with a witness share. No information was given as to _why_ this is the recommendation (I'd love to see that). Using MNS means it needs three nodes (instead of the two nodes typically seen in an active-passive cluster), but it does enable geographically dispersed clusters.

When it comes to disaster recovery (DR), there are only a couple key points to consider:

* Restoring VC data is just taking care of the database, using whatever methods you would normally use
* Restoring the VC services comes either as a cold standby (reinstalling VC and restoring the state files) or a warm standby (preinstalled but disconnected, with state files being synchronized on a regular basis)

The procedure for performing a failover with a cold standby is as follows:

1. Install a fresh instance of VC on the cold standby server.

2. Install the configuration files and connect to the standby database.

3. Run the Perl scripts to reconnect the ESX hosts, unless the standby server can assume the IP address of the primary VC server (Perl scripts available at [http://www.vmware.com/go/viperl/scripts](http://www.vmware.com/go/viperl/scripts)).

With a warm standby, it's only necessary to ensure that the configration/state files are kept synchronized and then run the Perl scripts (if necessary; see above).

The session wrapped up with some common issues and troubleshooting information, with nothing terribly unusual or outstanding (useful information, yes, but not incredibly groundbreaking). This included information like necessary network ports, network bandwidth, network latency, etc. (By the way, you can adjust the network latency timeout by editing the vpxd.cfg file on the VirtualCenter server.)
