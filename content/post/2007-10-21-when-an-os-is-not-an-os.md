---
author: slowe
categories: Musing
comments: true
date: 2007-10-21T12:42:16Z
slug: when-an-os-is-not-an-os
tags:
- ESX
- Microsoft
- Virtualization
- VMware
- Windows
title: When an OS is not an OS
url: /2007/10/21/when-an-os-is-not-an-os/
wordpress_id: 562
---

I'm not really sure how to respond to this article titled ["An OS is an OS is an OS"](http://blogs.msdn.com/volkerw/archive/2007/08/14/an-os-is-an-os-is-an-os.aspx). The author argues that [VMware ESX Server](http://www.vmware.com/products/vi/esx/) _is_ an operating system; therefore, their (VMware's) claims about bringing about the end of the OS is a bit hypocritical. My initial (admittedly knee-jerk) reaction was to exclaim, "No it's not!" and write something to that effect here.

Fortunately, I subdued the knee-jerk reaction, did some more thinking and reading, and have finally arrived at a point where I can see the author's point and agree with him---to a certain extent.

The strict definition of an "operating system" has dramatically changed over the years, and from that perspective VMware _does_ have a right to distinguish ESX Server from the typical OS. The critical question here is, "What exactly comprises an operating system?" [This Wikipedia article](http://en.wikipedia.org/wiki/Operating_system) (to which the original article's author linked) states that:

>"Operating Systems themselves have no user interfaces; the user of an OS is an application, not a person."

The heart of ESX Server is the vmkernel, which has no user interface and can't be observed or interacted with. By the strict definition of an OS according to Wikipedia, then you're right---the vmkernel is an operating system, and VMware provides the Service Console as the "application" by which users can interact with the vmkernel's managed resources. In this case, then the original author's complaint against VMware is valid---you can't say that you're going to eliminate operating systems by pushing your own operating system. An OS is an OS is an OS, even if those OSes are very different in design and purpose.

Over time, though, the perception of an "operating system" has grown to mean more than perhaps the strict Wikipedia definition really entails. If an operating system has no user interface, then how do we classify [Microsoft Windows](http://www.microsoft.com/windows/)? Or [Mac OS X](http://www.apple.com/macosx/)? In this strict context, Windows would be defined as an application that runs on top of "something else" (the Mach kernel and kernel-mode subsystems, I suppose; it's been ages since I took a hard look at the Windows internals) to provide access to managed resources. Back in the Windows 3.1 days, Microsoft got seriously torqued when people referred to Windows as "an MS-DOS application," but that's an accurate definition in the strict technical sense of the term. Today, I'm sure that calling Windows "an application" would get Microsoft equally torqued. (Note that Linux is not so plagued by this problem, since the X Window System is clearly an application that runs on top of the OS, as is the text-mode shell.)

Further complicating the matter is the announcement of [ESX Server  3i](http://www.vmware.com/products/vi/esx/esx3i.html), a slimmed down version of the hypervisor that can be embedded in server firmware. Is the argument still the same? Is the vmkernel still an OS then? If so, what is "the application" by which users will interact with the vmkernel? If we have broadened the definition of an OS to include the operating environment in which a user works (this would be the GUI in Windows, or the Aqua layer in Mac OS X), then the vmkernel no longer has that component. It has no significant user interface. Is it still an operating system?

&lt;aside&gt;It's true that persistent beta users have discovered the presence of a command-line interface on ESX Server 3i, but it's unclear at this time how pervasive the support for that CLI truly is.&lt;/aside&gt;

I suppose this all sounds like a pretty trivial discussion, but at a deeper level this goes back to some thoughts I had (along with others) about [the future of the OS][1]. In that article, I stated (sorry I'm quoting myself here):

>If you're an operating system guy, you'll say that the OS has a bright future, and point to developments such as built-in paravirtualization and bundled hypervisors to prove your point. If you're a virtualization guy, you'll say that the OS is dead, and you'll point to developments such as third-party paravirtualization and independent hypervisors to prove your point.

See, Microsoft _has_ to make the claim that ESX Server is an operating system, so that it can level the playing field---at least with regards to perception---between VMware's virtualization solution and Microsoft's own virtualization solution. After all, if you're VMware and you're saying that your solution runs "on the bare metal, beneath the OS," then this sounds somehow _better_ than Microsoft's hypervisor, which is "bundled with the OS." See the difference? If Microsoft allows the perception that ESX Server is not an OS to grow, then it undermines their solution.

Likewise, VMware _has_ to distinguish their solution from the "traditional OS" because if they don't, then _they_ begin to lose the perception advantage against other virtualization solutions such as Xen or Windows Server Virtualization. You can see this trend even now on the VMware web site, where you'll find statements like this about ESX Server 3i:

>ESX Server 3i is the only hypervisor that does not incorporate or rely on a general-purpose operating system (OS), eliminating many common reliability issues and security vulnerabilities.

Is ESX Server an "operating system"? What about ESX Server 3i? As with so many other things in life, I guess that truly depends upon your perspective.

[1]: {{< relref "2006-12-05-the-future-of-the-os.md" >}}
