---
author: slowe
categories: News
comments: true
date: 2007-04-06T11:11:33Z
slug: vmware-fusion-beta-3
tags:
- Apple
- Fusion
- macOS
- Virtualization
- VMware
title: VMware Fusion Beta 3
url: /2007/04/06/vmware-fusion-beta-3/
wordpress_id: 438
---

The development team at [VMware](http://www.vmware.com/) has released [Beta 3 of VMware Fusion](http://www.vmware.com/products/beta/fusion/), and I had the opportunity to download and install the new version earlier this morning. Based on what I've seen so far, this is a solid improvement over earlier versions and the development team is making good progress on the product.

New to this version of Fusion are some of the following features (more information available in the [release notes](http://www.vmware.com/products/beta/fusion/releasenotes_fusion.html)):

* Improved performance (debugging can be turned off)  
* Support for Boot Camp  
* Host-only networking support  
* Improved hardware editor

Particularly useful for me, coming from a [VMware ESX Server](http://www.vmware.com/products/vi/esx/) background, is the ability to access hardware settings directly from the Virtual Machine Library window (the main Fusion window). This was a key complaint of mine from earlier beta versions, and I'm glad to see it addressed now in Beta 3. Performance seems much better, which I'm sure is due to the ability to turn off debugging, but I'm also sure that there is still additional tuning to be done by the development team before the product reaches final release. It's pretty doggone fast now in my opinion, so I'm really excited to see how much faster it may get.

It's also nice to see the development team throw in "little" things like support for packages, a [Mac OS X](http://www.apple.com/macosx/)-specific thing that allows us to manipulate groups of files as a single item. (Mac users will know what I'm talking about.)

There are also little UI polishes here and there, like a fade in-fade out effect when suspending or resuming a VM, and graphic overlays on a VM window to run or resume a paused VM.

Users of previous versions, especially the very early releases (I've been using it since before public beta), beware of one problem  I've run into and reported back to VMware: the early sound device presented by Fusion may cause problems in this release. I had the UI for Fusion keep crashing due to this problem (the VM still runs in the background). Manually editing the VMX to remove the sound adapter and then re-adding the sound adapter in the UI fixed the problem. This will only be an issue for users of very early builds of Fusion who, like me, have kept the same VM throughout all the new builds.

If you are an Intel-based Mac user, you owe it to yourself to have a look at this product. This is _especially_ true if you have co-workers that also use VMware virtualization products, like [VMware Workstation](http://www.vmware.com/products/ws/), on Windows or Linux, as this allows you to move VMs between platforms with very little effort. Right now, that's an advantage that VMware has over its competition, and a definite plus in my book.
