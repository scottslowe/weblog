---
author: slowe
categories: Musing
comments: true
date: 2006-12-05T11:54:47Z
slug: the-future-of-the-os
tags:
- Apple
- ESX
- Linux
- Macintosh
- Microsoft
- Virtualization
- VMware
- Windows
title: The Future of the OS
url: /2006/12/05/the-future-of-the-os/
wordpress_id: 378
---

Over at [Virtualization Daily](http://www.virtualizationdaily.com/archives/85_the-os-is-under-attack.html), Kimbro Staken pulls together a variety of views about [the future of the operating system](http://www.virtualizationdaily.com/archives/85_the-os-is-under-attack.html). Citing such forces as Web applications, virtualization, and ever-increasing hardware power, he and others believe that the general purpose operating system as we know it has no future.

I do agree with these conclusions on at least one point: The general purpose operating system as we know it will cease to exist in the next 5 to 10 years, perhaps sooner. I do believe that the release of massive development projects such as [Windows Vista](http://www.microsoft.com/windowsvista/) won't be the norm moving forward and that, in fact (as others have predicted as well), Windows Vista will be the last of its kind.

Notice I didn't place [Mac OS X](http://www.apple.com/macosx/) in that list as well. Why? Because I think that [Apple](http://www.apple.com/) is capitalizing on an architecture and a convergence of technology that allows it to make Mac OS X into what Windows NT was _supposed_ to be. (Go back and read the stories about the development of Windows NT and look at Dave Cutler's vision for the operating system---an application environment-agnostic system in which OS/2, Windows, and POSIX-compliant applications could all run without modification.) Does that sound like anything else we have these days? With Mac OS X today, I can run native Macintosh applications, command-line UNIX applications (sometimes straight "out of the box", sometimes needing a quick recompile), and X11-based applications. Add in something like [WINE](http://www.winehq.com/) (the open source Win32 API implementation) and we gain the ability to run many (but not all) Windows applications. Add in a virtualization solution such as that created by [VMware](http://www.vmware.com/) or [Parallels](http://www.parallels.com/) and you gain the ability to run _any_ Windows application.

I alluded to this in an earlier post about how [virtualization on the Mac was heating up][1]. My purpose here is not to rehash these thoughts, but instead to place them into the context of Kimbro's thoughts (and the thoughts of others) with regards to the future of the operating system. Is the operating system dead? Is there a future for the operating system?

I guess that depends on how you define the operating system. I tend to agree that virtualization is the future. Operating systems as they exist today simply can't take full advantage of the powerful hardware that is coming out of [Intel](http://www.intel.com/) and [AMD](http://www.amd.com/), with even more powerful hardware just around the corner (think quad-core CPUs). This is especially true in the datacenter, where VMware's [Virtual Infrastructure](http://www.vmware.com/products/vi/) shines. In the datacenter, where multi-socket multi-core CPUs, gobs of RAM, and terabytes of SAN storage reside, virtualization (in my opinion) is a _given._ But what about on the desktop? Is it a foregone conclusion on the desktop as well?

I think so, and here is where I think that Apple and [Microsoft](http://www.microsoft.com/) have positioned themselves very differently. Microsoft claims to be working on a hypervisor (a virtualization engine, so to speak), but their architecture just isn't designed to really take advantage of that---not now, anyway. In the old days of Windows NT, when you had the Win32, OS/2, and POSIX subsystems running on top of the "operating system," then adding a virtualization engine there and then leaving the ability to create new "application subsystems" would have been great. Now, Windows is just too monolithic. That may all change with Longhorn, but it's still too early to tell just yet. Linux is in better shape, but still a bit immature in many regards. UNIX? Too fragmented still, although [Solaris](http://www.sun.com/software/solaris/)/[OpenSolaris](http://www.opensolaris.org/) is looking pretty good. Honestly, I think that Mac OS X is best positioned to take advantage of the virtualization wave. It wouldn't surprise me in the least (despite claims to the contrary from Cupertino) for Apple to buy Parallels and build virtualization right into the OS. Then, they have a platform (both hardware and software) that capable of running just about any application on the planet right out of the box. Windows applications sitting side by side with X11 and Aqua (native Mac) applications? No problem. On the desktop side, this seems to be a much more likely scenario.

So I guess the future of the operating system depends on your perspective. If you're an operating system guy, you'll say that the OS has a bright future, and point to developments such as built-in paravirtualization and bundled hypervisors to prove your point. If you're a virtualization guy, you'll say that the OS is dead, and you'll point to developments such as third-party paravirtualization and independent hypervisors to prove your point. Which of these two is correct?

[1]: {{< relref "2006-12-04-macintosh-virtualization-heating-up.md" >}}
