---
author: slowe
categories: Information
comments: true
date: 2008-02-08T14:23:54Z
slug: virtualization-short-take-1
tags:
- iSCSI
- NFS
- Storage
- Virtualization
- VMware
title: 'Virtualization Short Take #1'
url: /2008/02/08/virtualization-short-take-1/
wordpress_id: 626
---

As sometimes happens, I've been collecting a variety of virtualization-related links that, while interesting, don't necessarily warrant a full blog posting by themselves. Since I don't want to just copy someone else's content and not provide any value of my own, I've decided to start irregularly publishing a "Virtualization Short Take". Each of these will just be a few links with a brief commentary or thought attached. Perhaps these will lead to broader and more active discussions around some of these topics.

Without further delay, then, here's the first-ever Virtualization Short Take:

* Via Duncan, a new and [undocumented feature in VCB's config.js](http://www.yellow-bricks.com/2008/02/06/undocumented-vcb-configjs-feature/) file that allows for non-quiesced snapshots of virtual machines. I'd be interested in more discussion around why some workloads might be better served by non-quiesced snapshots, so if anyone has some insight please speak up.

* Also via Duncan, fixes for crashes caused by installing the Converter plugin to VirtualCenter. Two solutions are available; try [this one](http://www.yellow-bricks.com/2008/02/04/installing-the-converter-plugin-crashes-virtualcenter/), then try [this approach](http://www.yellow-bricks.com/2008/02/05/installing-the-converter-plugin-crashes-the-virtualcenter-client-part-ii/).

* Via Thomas, it looks like VMware has [published a storage performance white paper](http://scalethemind.com/2008/02/vmware-releases-storage-protocol-performance-white-paper/) comparing Fibre Channel, hardware iSCSI, software iSCSI, and NFS. I just finished reviewing the document myself, and plan to go back and look at it again more thoroughly. It's useful information for virtualization architects or engineers responsible for designing storage solutions for VMware.

* In case anyone's interested in creating their own bootable ESX 3i USB drive, here's [more information](http://www.amikkelsen.com/?p=48). This is something I need to take a closer look at myself, so when I have the chance to run through these instructions I'll try to post some feedback on how well they worked.

* Based on some interaction with a customer yesterday, it looks as if VirtualCenter 2.5 may require the Power User role to be directly assigned to the Hosts & Clusters object in order for users to be able to attach local (client-side) ISO files to a VM. Has anyone else seen this behavior? I'm going to try to recreate the problem myself, but if you've seen this please speak up in the comments.

* I've also recently received word from a friend that the thin-provisioned disks VMware uses by default on an NFS datastore may not be honored when cloning VMs from a template. Again, I plan to test this myself, but if anyone else out there has seen this behavior I'd love to hear about it.

As always, I'd love to hear your thoughts, so feel free to add your voice in the comments below.
