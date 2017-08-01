---
author: slowe
categories: Information
comments: true
date: 2008-07-31T16:26:50Z
slug: virtualization-short-take-15
tags:
- ESX
- ESXi
- NetApp
- Virtualization
- VMotion
- VMware
- VMwareHA
title: 'Virtualization Short Take #15'
url: /2008/07/31/virtualization-short-take-15/
wordpress_id: 773
---

A whole lot has been buzzing around in the virtualization space over the last week or so, a lot of it as a result of the release of VMware Infrastructure 3 version 3.5 Update 2 (what a mouthful!). Anyway, here are some links I found interesting or about which I wanted to comment.

* I found an article discussing [the use of NetApp FlexClones and deduplication along with VMware Infrastructure](http://markarnold.blogspot.com/2008/07/vmware-server-virtualization-and-vdi.html) for virtualization. The author of the blog post does a good job of pointing out some of the reasons why one would want to use FlexClones vs. deduplication in various scenarios, but doesn't go far enough (in my opinion) in helping readers understand that using FlexClones isn't exactly the "walk in the park" that NetApp makes them out to be. I've discussed the use of FlexClones extensively here; just take a look at [how to provision VMs using FlexClones][1], [part 1][2] and [part 2][3] of advantages/disadvantages of FlexClones with VMware, and [LUN clones vs. FlexClones][4]. FlexClones _can_ be useful, but deduplication is far more useful, in my opinion, simply because it doesn't require any configuration at the virtualization layer.

* Paul Shannon at VM-Aware has a PDF on [how to hot extend virtual disks with Update 2](http://www.vm-aware.com/2008/07/29/how-to-hot-extend-virtual-disks-using-esx-35-update-2/). I personally had a problem reading the PDF; the pictures were there but there was no text. Even so, the pictures were helpful in understanding the process. As expected, this functionality does _not_ address Windows-specific issues, like extending partitions on the newly expanded virtual disk.

* It looks like VirtualCenter 2.5 Update 2 introduces a case-sensitivity bug. Duncan and Rick pick up on this problem; Duncan's post is [here](http://www.yellow-bricks.com/2008/07/28/esx-35-u2-and-ha-error/) and Rick's post is [here](http://www.vmwarewolf.com/ha-problem-with-virtualcenter-25-update-2/). The key, apparently, is to make sure your hostnames are lowercase everywhere. That's kind of a habit I developed years ago, so I think I'm covered, but it's useful to know nevertheless.

* Is VMotion a bad idea? That's [the question posed here](http://www.cio.com/article/439487/Is_One_of_VMware_s_Best_Features_a_Really_Bad_Idea_?page=3) by Kevin Fogarty. I can see the crux of the argument against it; why expose yourself to "unknown risks"? The _real_ question here, though, is this: what is the risk of performing a VMotion? The author of the article seems to imply that an application might crash as a result of a live migration. Personally, I have never had an application crash as the result of a VMotion operation. I've done VMotion operations with file servers, streaming media servers, web servers, terminal servers, X11 sessions, Telnet and SSH sessions, and just about anything else I can think of. I have yet to even drop a session, much less have an application crash. Is the possibility there? Sure, I suppose. But as IT professionals we have to plan and design according to our experience, and in my experience VMotion is not an "unknown risk." Of course, there are many environments in which I've not tested VMotion, so I'll toss this back to the readers: have you ever seen an application crash as a result of VMotion? Would you consider VMotion an "unknown risk"?

* Duncan, sharp-eyed individual that he is, [caught](http://www.yellow-bricks.com/2008/07/29/high-availability-change/) that as of Update 2 the default isolation response is now set to "Leave powered on". In addition, Update 2 introduces "Shutdown VM," which initiates an orderly shutdown of the guest in reaction to host isolation. Good catch, Duncan!

* It's been a couple of weeks, but Christofer Hoff [weighed in](http://rationalsecurity.typepad.com/blog/2008/07/on-the-utility.html) on some [criticisms of the CIS benchmark](http://cio.com/article/422513/CISecurity_Guide_to_VMware_Security_Falls_Far_Short) for VMware ESX. While Edward's comments are valid, so are Hoff's; benchmarks have to be taken for what they are---they are guidelines, nothing more, and can only be so useful as such. Were this an application that purported to make your servers secure, then the criticisms would be much more valid. Security is more like a trip rather than a destination, and these guidelines are just another landmark along the way. (Hey, that was pretty good.)

* Duncan's been on a bit of a roll, with this post on [using the VMware Converter plugin to schedule P2V conversions](http://www.yellow-bricks.com/2008/07/30/cool-feature-of-the-vmware-converter-plugin/) as a means of backup and this one pointing out [a PDF on how to install ESXi 3.5 Update 2 on a USB key](http://www.yellow-bricks.com/2008/07/29/esxi-35-update-2-on-a-usb-memory-key/).

I guess that's about it for this week. Thanks for reading!

[1]: {{< relref "2007-05-11-how-to-provision-vms-using-netapp-flexclones.md" >}}
[2]: {{< relref "2007-05-15-netapp-flexclones-with-vmware-part-1.md" >}}
[3]: {{< relref "2007-05-17-netapp-flexclones-with-vmware-part-2.md" >}}
[4]: {{< relref "2007-05-21-lun-clones-vs-flexclones.md" >}}
