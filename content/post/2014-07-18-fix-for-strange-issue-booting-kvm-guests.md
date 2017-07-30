---
author: slowe
categories: Explanation
comments: true
date: 2014-07-18T09:00:00Z
slug: fix-for-strange-issue-booting-kvm-guests
tags:
- KVM
- Linux
- Virtualization
title: Fix for Strange Issue Booting KVM Guests
url: /2014/07/18/fix-for-strange-issue-booting-kvm-guests/
wordpress_id: 3472
---

This issue describes a fix I found for an issue I had when booting KVM guest domains on the Ubuntu/KVM hypervisors in my home lab. I'd been struggling with this issue for quite some time now, but only recently found what I believe to be the final fix for the problem.

First, allow me to provide a bit of background. Some time ago---I'd say around August 2012, when I left the vSpecialist team at EMC to join an OpenStack-focused team in another part of EMC---I moved my home lab over completely to Ubuntu 12.04 LTS with the KVM hypervisor. This was an important step in educating myself on Linux, KVM, libvirt, and Open vSwitch (OVS), all of which are critical core components in most installations of OpenStack.

Ever since making that change---particularly after adding some new hardware, a pair of Dell C6100 servers, to my home lab---I would experience intermittent problems booting a KVM guest. The guest would appear to boot properly, but then hang shortly after a message about activating swap space and fsck reporting that the file system was clean. Sometimes, rebooting the guest would work; many times, rebooting the guest didn't work. Re-installing the guest sometimes worked, but sometimes it didn't. There didn't appear to be any consistency with regard to the host (the issue occurred on all hosts) or guest configuration. The only consistency appeared to be with Ubuntu, as virtually (no pun intended) all my KVM guests were running Ubuntu.

Needless to say, this was quite frustrating. I tried all the troubleshooting I could imagine---deleting and recreating swap space, manually checking the file system(s), various different installation routines---and nothing seemed to make any difference.

Finally, just in the last few weeks, I stumbled across [this page](http://forum.proxmox.com/threads/17997-ubuntu-12-04-KVM-Error-during-boot-never-finishing-boot-from-console), which indicated that adding "nomodeset" to the grub command line fixed the problem. This was a standard part of my build (it kept the console from getting too large when using VNC to connect to the guest), but it required that I was able to successfully boot the VM first. I'd noted that once I had been able to successfully boot a guest and add "nomodeset" to the grub configuration, I didn't have any further issues with that particular guest; however, I explained that away by saying that the intermittent boot issue must have been some sort of first-time boot issue.

In any case, that page linked to [this ServerFault entry](http://serverfault.com/questions/561286/why-is-my-machine-not-showing-anything-when-booting), which also indicated that the use of "nomodeset" helped fix some (seemingly) random boot problems. The symptoms described there---recovery mode worked fine, booting normally after booting into recovery mode resulted in an "initctl: event failed" error---were consistent with what I'd been seeing as well.

So, I took one of the VMs that was experiencing this problem, booted it into recovery mode, edited the `/etc/default/grub` file to include "nomodeset" on the `GRUB_CMDLINE_LINUX_DEFAULT` line, and rebooted. The KVM guest booted without any issues. Problem fixed (apparently).

Thus far, this has fixed the intermittent boot issue on every KVM guest I've tried, so I'm relatively comfortable recommending it as a potential change you should explore if you experience the same problem/symptoms. I can't guarantee it will work, but it has worked for me so far.

Good luck!
