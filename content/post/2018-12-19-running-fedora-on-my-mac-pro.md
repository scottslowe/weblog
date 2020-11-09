---
author: slowe
categories: Explanation
comments: true
date: 2018-12-19T12:00:00Z
tags:
- Linux
- Apple
- Hardware
- Fedora
title: Running Fedora on my Mac Pro
url: /2018/12/19/running-fedora-on-my-mac-pro/
---

I've been working on migrating off macOS for a couple of years (10+ years on a single OS isn't undone quickly or easily). I won't go into all the gory details here; see [this post for some background][xref-1] and then see [this update from last October][xref-2] that summarized my previous efforts to migrate to Linux ([Fedora][link-1], specifically) as my primary desktop operating system. (What I _haven't_ blogged about is the success I had switching to Fedora full-time when I joined Heptio.) I took another big step forward in my efforts this past week, when I rebuilt my 2011-era Mac Pro workstation to run Fedora.<!--more-->

When I mentioned this on Twitter, a few people asked the question every parent dreads hearing: "Why?" (If you've been a parent for more than a couple years you'll understand this.) The motivation for using Linux is something I've already discussed. As for the hardware, it's simple: the hardware for the Mac Pro is very good (see the base specs [here][link-2]), so why _not_ re-use the hardware for use with Linux? I mean, if I've already decided on running Linux (which I have), then why spend money on new hardware when the hardware I have meets my requirements?

Anyway, once I'd migrated all my data off---which itself was no small feat, and perhaps worthy of a separate blog post of its own---I installed Fedora 28 via a USB drive I created using Fedora Media Writer on my MacBook Pro (that's right, I haven't _completely_ eradicated macOS just yet). The installation seemed to go fine, but in reality I encountered a number of issues after it completed:

* The installer didn't set the hostname (I admit that I may have missed this during the installation process).
* I would get `sss_cache` messages along the lines of `confdb_get_domains error` anytime I tried to manipulate users or groups; turns out the system's `sssd` configuration was mangled (in comparison to my laptop's configuration) with missing services, services not enabled or running, etc.
* As of Fedora 28, the installer no longer creates an account during the installation. Unfortunately, the user creation process post-installation doesn't expose all the options, so I couldn't set a consistent UID and GID across my Fedora systems. (This probably doesn't matter in the long run, but I still prefer to keep them consistent.)

After fighting with the Samsung SSD I was using as my boot drive in the Mac Pro (and finally accepting that there would be an additional EFI System Partition on the drive that would apparently go unused), I re-installed Fedora, this time using Fedora 27 bootable media (again, created using Fedora Media Writer). This time the installation was _much_ smoother, and the resulting system build was _much_ more stable. After a not-so-quick `dnf update` followed by an even-longer `dnf system-upgrade` (to upgrade to Fedora 28), I was back to Fedora 28, but this time without any of the issues I'd experienced earlier. I don't know the source of the problems with the original F28 installation, but re-installing using F27 and then upgrading to F28 worked better than a fresh F28 installation. Go figure!

Overall, most everything works as expected. There are a few oddities:

* I'm using the Mac Pro's digital audio out (S/PDIF) to connect to a Samsung sound bar under the monitors. I guess Fedora doesn't send enough information to the sound bar, because it will turn itself off after a short after no sound being sent to it. It turns itself back on when the system sends sound, so it's more of an annoyance than anything else.
* The built-in Wi-Fi adapter doesn't work (but this is a known issue with most Apple hardware, and can be resolved if needed), but I don't need it so it doesn't really matter.
* I'm not using an Apple keyboard so I didn't have an Eject key on the keyboard to open/close the Mac Pro's optical drive (which _is_ recognized by Fedora). CLI to the rescue---just use `eject -T /dev/sr0` to toggle the drive's open/close status.

Otherwise, Fedora seems to run just fine. I am seeing some kernel-related errors on boot that I need to track down, but those errors don't seem to be translating into issues actually using the system (I'm writing this blog post on the Fedora-powered Mac Pro right now).

As a bonus, the Micron p420m PCI SSD drive that wasn't recognized under macOS works perfectly under Fedora. Now to get Libvirt configured to use that for all VM-related storage...

If you have any questions or want more information, feel free to [hit me up on Twitter][link-3].

[link-1]: https://getfedora.org/
[link-2]: https://everymac.com/systems/apple/mac_pro/specs/mac-pro-eight-core-2.4-mid-2010-westmere-specs.html
[link-3]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2012-11-30-why-i-might-leave-os-x.md" >}}
[xref-2]: {{< relref "2017-10-25-linux-migration-wrap-up.md" >}}