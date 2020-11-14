---
author: slowe
categories: News
comments: true
date: 2007-03-02T18:03:36Z
slug: fusion-beta-2
tags:
- Fusion
- macOS
- Virtualization
- VMware
title: Fusion Beta 2
url: /2007/03/02/fusion-beta-2/
wordpress_id: 422
---

With [everyone](http://www.powerpage.org/2007/03/vmware_releases_beta_2_of_fusion.html) [and](http://blogs.vmware.com/vmtn/2007/03/drumroll_please.html) [their](http://tarrysingh.blogspot.com/2007/03/vmware-fusion-beta-2-rolls-out-with.html) [brother](http://www.virtualizationdaily.com/archives/125_vmware-fusion-for-mac-os-x-beta-2-released.html) [reporting](http://www.tuaw.com/2007/03/02/vmware-fusion-beta-2-with-experimental-3d-graphics/) the release of Fusion Beta 2, I'm hardly on the "cutting edge" with this posting. To be honest, I don't have the time to be on the leading edge of news postings, so rather than just regurgitate the same old stuff again here, I thought I might speak briefly as to the features that matter most to me as an enterprise IT consultant.

Here are the features that matter most to me:

* Virtual battery support: This is a handy one because it helps my MacBook Pro's battery last longer. Previously, the Windows guests were not aware of the power status of the host, so they just ran full throttle and drained the battery in no time flat. Now, the Windows guests can be aware of the power status of the host and adjust their settings accordingly. _Very handy._

* Improved networking support: The added support for AirPort networking is a definite plus; I can't tell you how many times I got bit by that one while sitting in a conference room. Now I can use my Windows, Linux, or Solaris VMs across the WLAN while I'm in the conference room. Again, very handy. Although I still haven't found any sort of advanced UI that lets us replicate the same kind of functionality as found in VMware Workstation (to create virtual switches and bridges), I'm hoping that will come soon.

* Snapshots: I'm _really_ glad to see this. I don't use VM snapshots very often, but this is one feature that really differentiates Fusion from the Parallels offering. It also reassures me that Fusion isn't going to be just a "consumer level" offering but that it will retain at least some of the professional-class features for which VMware Workstation is so well known.

Honestly, I don't really care about the accelerated 3-D support; I very rarely play games on my Mac. If I were a serious gamer and a serious Mac-lover at the same time, I'd probably have a separate gaming system for that. Still, I imagine there are some graphics professionals out there for whom the graphics acceleration will be extremely useful.

So what do I still want to see in Fusion? Expanded snapshot functionality approaching that of VMware Workstation and enhanced networking support (to help facilitate guest-to-guest communications with separately defined networks, plus NAT, host-only, and bridged networking). Performance is pretty good now, and I'm sure it will only get better as the code is further reviewed and optimized during the beta/release candidate stages.

What's still missing, though, is that one _killer feature_ that would really set Fusion apart from any other Mac virtualization solution. The problem is, I don't know what that killer feature is. Is it the equivalence of Parallel's Coherence feature? Is it the enhanced snapshot/linked clones functionality of VMware Workstation brought over to the Mac? Or is it something else entirely? If you're so inclined, tell me what you think Fusion's missing "killer feature" should be in the comments.

In the meantime, jump over to VMware's site and [get the latest build of Fusion](http://www.vmware.com/products/beta/fusion/).
