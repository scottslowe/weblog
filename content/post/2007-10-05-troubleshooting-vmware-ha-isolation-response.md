---
author: slowe
categories: Explanation
comments: true
date: 2007-10-05T16:47:03Z
slug: troubleshooting-vmware-ha-isolation-response
tags:
- ESX
- NFS
- Virtualization
- VMware
- VMwareHA
title: Troubleshooting VMware HA Isolation Response
url: /2007/10/05/troubleshooting-vmware-ha-isolation-response/
wordpress_id: 554
---

This article started life as something entirely different. I was reviewing some of the VMworld 2007 slide decks, looking for "nuggets of knowledge," as I like to call them (these are the small details that are often far more significant than they might seem) when I came across some information on VMware HA isolation response. I was actually looking for something else but as is typically the case when you're looking for something, you find everything but the one thing for which you're looking.

In any event, I wanted to take some time to better understand isolation response, so I decided to perform some experiments in my lab with VMware HA and isolation response. For those that aren't familiar with it, _isolation response_ is the term used to describe what an ESX Server in a DRS/HA cluster will do if it loses connectivity to all the other servers in the cluster, i.e., if it becomes isolated. Isolation response is set on a per-VM basis, and the default (I believe) is to power off. What this means is that when an ESX host becomes isolated, it will power off the VMs that are currently running on that host.

There's a great deal of debate as to whether this is the right setting or not, which I won't really delve into right now. In any case, how does a host determine if it is isolated, or if the rest of the cluster is just down? That's what got me started down this path. The VMware HA agent (which is really the Legato Automated Availability Manager, or AAM, agent---hence the AAMClient stuff in `esxcfg-firewall`) uses the Service Console's default gateway as its _isolation address_. Basically, what this means is that if a host can't get to any of the other hosts in the cluster **and** can't get to the isolation address, then it assumes it is isolated and initiates the isolation response. If it can't get to other nodes in the cluster but **can** reach the isolation address, then it is not isolated and should continue operation (perhaps even restarting some VMs locally since this would indicate host failures in the cluster).

The stuff I found in the VMworld 2007 slides talks of using a second isolation address, which provides the VMware HA agent with another means of verifying isolation before initiating the isolation response. Before I proceeded with setting this second address, however, I wanted to be sure I understood the operation of isolation response in the current configuration. Once I'd tested that and then tested the second isolation address, I was going to write it up here.

To make a long story not quite so long, I found that isolation response was _not_ working as expected. What happened is that other hosts in the cluster would detect the "host failure" (the isolation of my test host) and try to restart the VM before the test host detected isolation and tried to shutdown the VM. This was evidenced by these lines in `/var/log/vmkernel`:

	Oct  5 13:13:36 esx02 vmkernel: 38:20:29:34.025 cpu3:1305)WARNING: NFSLock: 1883: disk is being locked by other consumer  
	
	Oct  5 13:13:36 esx02 vmkernel: 38:20:29:34.025 cpu3:1305)NFSLock: 2479: failed to get lock on file vswim01-flat.vmdk 0x5a1b6a0 on 192.168.31.51 (192.168.31.51)

(Yes, I'm running my VMs on NFS. Yes, I did try iSCSI to see if the behavior was different. No, I did not try Fibre Channel. Yes, I got the same results in both cases.)

To make things even more interesting, I found that the test host failed to successfully shut down a Linux VM when the isolation response was finally triggered, but was able to successfully power down a Windows guest. Both VMs had the latest version of the VMware Tools installed.

Since that time, I've been combing the Internet searching for more information on the VMware HA agent, the AAM `ftcli` utility, behaviors, workarounds, configuration tweaks, etc. Thus far, it has been an abysmal failure. There are lots of VMware Community threads, but almost every one of those is a "double-check your DNS and /etc/hosts" thread.

So, any VMware gurus out there have some useful information to share? Anyone else having VMware HA problems? Anyone know where I can find some actually useful information on VMware HA and the AAM client? I'd love to get some more detailed information and be able to put this thing to rest (and be able to advise others on how to put it to rest as well).
