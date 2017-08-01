---
author: slowe
categories: Musing
comments: true
date: 2006-12-26T22:38:28Z
slug: application-agnosticism
tags:
- Apple
- Macintosh
- Virtualization
- VMware
title: Application Agnosticism
url: /2006/12/26/application-agnosticism/
wordpress_id: 391
---

I coined the term "application agnosticism" in the context of the discussion surrounding virtualization and its impact on the future of the operating system. Virtualization proponents, such as [VMware](http://www.vmware.com/), say that virtualization means the end of the general purpose operating system and instead point to the rise of virtual appliances. In the virtual appliance realm, the operating system exists to provide services to the application, and unnecessary components of the operating system are stripped away. This is a good solution in the datacenter, but is it an equally valid approach on the desktop?

My view is that the forces that shape the desktop market are going to push operating systems and OS functionality in a different direction. This is not to say that virtual appliances don't have a place in the personal desktop world, just that they won't have the same level of importance. This is also not to say that virtualization doesn't have a place in the personal desktop world. Virtualization _does_ have a place and _will_ play an important role moving forward. The functionality of virtualization will just be put to use in a different way.

Simply put, application agnosticism is the ability for a general purpose operating system (such as Windows, Linux, or Mac OS X) to run any kind of application without regard to operating system for which that application was designed. Want to run Linux applications on Windows? Or Windows applications on Mac OS X? Application agnosticism would allow that functionality. There are a number of components that play into making application agnosticism possible:

* Convergence on x86 hardware: For better or worse, the hardware industry is converging on x86-compatible hardware. (Fortunately, we aren't tied to just one vendor in that regard, but that's another story for another day.) This means that coding for many different platforms isn't as necessary today, nor is it quite as likely to have applications written for many different CPU platforms.

* Application-specific subsystems: Like the POSIX subsystem in Windows NT and successive generations, the OS/2 subsystem in early Windows NT generations, or the X11 application in Mac OS X today, these application-specific subsystems provide the necessary resources to run applications not specifically designed to run on that particular OS. Windows seems to be moving away from this functionality, having removed the OS/2 subsystem and (I believe) the POSIX subsystem as well. Mac OS X, on the other hand, seems to be moving strongly in this direction, with the BSD subsystem and X11 support. API emulators, such as WINE, are also forms of application-specific subsystems (more on that in a moment).

* Virtualization: Virtualization is a key enabling technology for application agnosticism. As vendors such as VMware, Parallels, and Microsoft move to provide greater integration between the host and guest environments, this role becomes more evident. Excellent examples of this type of host-guest interaction are the drag-and-drop file sharing of VMware's Fusion beta, the Coherence feature in Parallels Desktop for Mac, and the ability of the now-defunct Microsoft Virtual PC for Mac to launch the PC guest environment when a user double-clicked on a PC file type in the host environment.

In the datacenter, these kinds of host-guest interactions are not only unnecessary, but actually undesired---very few would actually _want_ the ability to drag and drop files between a host server (assuming there's actually a host OS present) and a guest server, especially if that guest server is running in a "headless"-type scenario in the background. On the desktop side, however, these kinds of interactions are quite useful, and help extend the desire and ability of users to actually make use of these kinds of technologies. It's these kind of forces that I believe will drive virtualization on the desktop in a different direction than virtualization on the server, and what will bring about application agnosticism.

In the initial discussions of application agnosticism, I mentioned that I believed Mac OS X to be further along the curve to embracing application agnosticism than other general purpose operating systems. If you look at the components that make application agnosticism possible, it appears to me that Apple embraces and utilizes more of these components than some of the other major operating systems on the market. Without this devolving into a discussion of one operating system versus another or which OS is better, allow me to mention the ways in which this appears to be the case:

1. Mac OS X currently utilizes application-specific subsystems:

   * There is a BSD subsystem that allows many UNIX command-line applications to run without modification. Some require a simple recompile in order to run. Future versions of Mac OS X are seeking certification as an "official" flavor of UNIX from The Open Group, further increasing the compatibility and usefulness of this subsystem and its ability to run command-line UNIX applications.

   * Likewise, there is an X11 subsystem for Mac OS X as well, allowing users to run native Mac OS X applications side-by-side with X11 applications. Of course, this is certainly not an advantage possessed solely by Mac OS X as there are X11 subsystems available for Windows, and X11 is "native" to Linux distributions.

   * Future versions of Mac OS X (the next version, code-named "Leopard," in particular) may include technologies developed years ago by Apple. Called "Red Box" at the time, these technologies would allow Mac OS X to run Windows applications at near-native speeds. Like the Intel version of Mac OS X that was buried for years before resurfacing at the start of the Intel transition, it's very possible that Red Box could rise again in Leopard and enable Mac users to run Windows applications on their Macs. A number of others have written about this possibility; see [here](http://www.gigoblog.com/2006/12/12/apples-virtualization-master-plan) and [here](http://macdailynews.com/index.php/weblog/comments/6110/) for more information. And let's not discount WINE, which currently runs on Linux and is being ported to Mac OS X. Both of these could be considered application-specific subsystems designed to support Windows applications on Mac OS X.

2. Mac OS X seems to be at the forefront of host-guest integration:

   * Parallels Desktop for Mac offers a Coherence mode, which I mentioned earlier in this article as well as [here][1]; this allows you to run virtualized applications side-by-side with host applications. No separate desktop environment, or windows inside another window. Just applications running.

   * VMware's Fusion product, [now in public beta][2], offers drag-and-drop interaction between the host and guest environments. Again, drag-and-drop is a function that users on the desktop have come to expect, so embedding this into a virtualization solution makes perfect sense.

   * The now-defunct Virtual PC for Mac, from Microsoft, offered a number of very innovative host-guest integrations during its lifetime. I mentioned guest file type registration already, in which guest file types that were double-clicked in the host environment caused the guest environment to launch and load the document. If I'm not mistaken, Virtual PC also offered clipboard integration as well. (I never used Virtual PC on the Windows platform, so these features may have been present or still be present in that version.)

Take a moment and think about an environment utilizing a virtualization engine with all three of the functional integrations I described above---drag-and-drop between host and guest, side-by-side host/guest windows in the same windowing system, and the ability to double-click a file in the host and have it launch in a guest. In that kind of environment, a user could easily run just about any application for just about any operating system and not have to really worry about the details. (Of course, this glosses over little details like installing the guest operating systems and the applications for those respective guest operating systems.) Being able to do that is what application agnosticism is all about. In my humble opinion, application agnosticism is virtualization's "killer app" on the desktop.

So there's my thoughts. What about you? Do you buy into the idea of application agnosticism? Or do you think that virtualization on the desktop is headed in a different direction? Speak up in the comments and let me know.

[1]: {{< relref "2006-12-04-macintosh-virtualization-heating-up.md" >}}
[2]: {{< relref "2006-12-22-vmware-fusion-public-beta.md" >}}
