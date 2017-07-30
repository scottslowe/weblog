---
author: slowe
categories: Information
comments: true
date: 2007-07-18T10:04:32Z
slug: assorted-vmware-tools
tags:
- Virtualization
- VMware
title: Assorted VMware Tools
url: /2007/07/18/assorted-vmware-tools/
wordpress_id: 490
---

Over the past few weeks, a number of VMware-related tools have been released. All of these tools are third-party tools written by avid [VMware](http://www.vmware.com/) fans or ISVs, and as far as I am aware all of these tools are available at no cost.

* First up is [VMX Extras](http://vmware.com/community/message.jspa?messageID=687637), a tool for editing the VMX files used by [VMware Fusion](http://www.vmware.com/beta/fusion/) (VMware's [Mac OS X](http://www.apple.com/macosx/) product). While you certainly can edit these files with a text editor (and I'm sure many of you have in the past), this makes it a bit easier. In addition, there are some preconfigured options available as well for stuff like a BIOS delay (to make accessing the BIOS of a VM easier), enabling paravirt-ops (for guests that support it, currently only Ubuntu 7.0.4 and derivatives), and some others.

* Next is [VCplus](http://www.run-virtual.com/?page_id=184), a tool released by Richard Garsthagen of [run-virtual.com](http://www.run-virtual.com/). Richard has a number of tools available on his site, including [VMotion Info](http://www.run-virtual.com/?page_id=155) (for helping to determine CPU compatibility for VMotion), [VM Time](http://www.run-virtual.com/?page_id=157) (for checking for time differences between guest and host), and the [Virtual MAC Tool](http://www.run-virtual.com/?page_id=173) (for assigning fixed MAC addresses to ESX/VirtualCenter guests). VCplus extends the functionality of VirtualCenter to include information such as disk usage inside the VM and if a VM has a snapshot (and if so, the size of the snapshot disk), and allows you to sync the DNS name of the VM to the display name. More functionality is planned, so this is one to keep your eyes on.

* Russian company [Veeam](http://www.veeam.com/) has been pushing out the VMware-related utilities, and the latest is one called EsxDiag. I was unable to find a link to the product on Veeam's website; I was alerted to EsxDiag [via virtualization.info](http://www.virtualization.info/2007/07/tool-esxdiag.html) and there is a link there to the application download.

* Eric Sloof has released a beta version of [his VMware MKS Client](http://www.ntpro.nl/blog/archives/170-The-games-have-begun.html), which is a remote console application for VMs. I haven't tried this one yet, but it sounds like it might be handy.

I'd love to hear feedback from anyone actually using some of these tools already in their environments. I have a few customers who could really use some of the functionality offered by some of these tools, but wouldn't want to recommend them until I get some feedback on real-world usage.
