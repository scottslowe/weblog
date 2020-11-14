---
author: slowe
categories: Musing
comments: true
date: 2007-09-27T23:49:19Z
slug: apple-and-vmwareor-xen
tags:
- Apple
- Citrix
- Fusion
- HyperV
- iPhone
- macOS
- Virtualization
- VMware
- Xen
title: Apple and VMware...or Xen?
url: /2007/09/27/apple-and-vmwareor-xen/
wordpress_id: 549
---

For a company that wants their virtualization technology to be ubiquitous, it would seem to make sense that VMware needs it to run on every major host operating system. Right now, VMware has Windows and Linux covered. But what about OS X? There is a tremendous amount of attention being paid to OS X right now, from many different sides, and [Apple](http://www.apple.com/) seems to be pushing OS X in a number of different directions (such as using OS X as the basis for the [iPhone](http://www.apple.com/iphone/), and rumors of future OS X-based iPods circulating). In my mind, it seems to make a lot of sense that both Apple and VMware could benefit from a closer relationship.

Think about it: Extending VMware ACE to include Mac OS X would now mean that VMware could have secure VMs running on pretty much any significant x86-based operating system from any significant manufacturer. The endpoint becomes irrelevant. Have a contractor that runs OS X? Not a problem, we can extend a secured, policy-controlled VM to his/her Mac laptop without any issues. Pocket ACE in action with all three major x86 host operating systems covered means that you can truly take your computing environment anywhere. It's a powerful thought.

Similarly, bringing VMware Player to Mac OS X gives Mac users out there exposure to the same wide range of virtual appliances that Windows and Linux users can currently access.

Not to be left out, I'm sure there are Xserve users out there that would love to have VMware's mature hosted virtualization technology running on their Mac OS X Server-based systems in the form of an OS X version of VMware Server. Anyone care to run OS X Server, Windows Server, and Linux all on a single piece of hardware in your datacenter?

"Wait a minute, Scott," you say. "Apple won't let us virtualize Mac OS X."

Who said anything about virtual OS X? You're right, of course; Apple has yet to budge on that front. However, that thought does lead me to my next thought: what will Apple do if VMware (or Parallels) doesn't provide the virtualization technology for their platform?

Apple has a history of integrating open source projects into Mac OS X; consider the FreeBSD-based underpinnings, the Apache web server, the Postfix mail server, and so forth. What's to stop Apple from integrating the Xen hypervisor?

Sun is integrating Xen as xVM; Microsoft's Windows Hypervisor (which is now available for public preview, by the way---I plan to have a look at it very soon) bears many architectural similarities to Xen, and of course Citrix will be using Xen in some significant way now that it's purchased XenSource for $500M. Why not Apple? Why not integrate Xen into the Apple code base? Apple can integrate Xen into their code base, release the open source bits as part of Darwin, and create their own virtualization solution. Apple controls the hardware base, after all, so it wouldn't be all that terribly difficult to write Xen-optimized drivers for alternate operating systems running under Mac OS X. I would imagine it would also be much easier to control the virtualization of Mac OS X if it were occurring on a version of OS X with Xen integrated.

So am I just crazy? Tell me what you think.
