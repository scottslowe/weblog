---
author: slowe
categories: Rant
comments: true
date: 2008-09-10T10:00:25Z
slug: the-changing-face-of-virtualization
tags:
- Apple
- Fusion
- macOS
- Virtualization
- VMware
title: The Changing Face of Virtualization
url: /2008/09/10/the-changing-face-of-virtualization/
wordpress_id: 880
---

While browsing the list of Virtual Strategy Magazine's list of virtualization blogs, I came across the link for [x86virtualization.com](http://x86virtualization.com), a site that I have browsed from time to time in the past. On this particular occasion, one particular article caught my attention:

**[Concerns Over VMware Fusion 2.0 Beta 2 and Parallels Desktop:](http://x86virtualization.com/desktop-computing/concerns-over-vmware-fusion-20-beta-2-and-parallels-desktop.html#more-315)**

>VMware touts the updates to their newest beta release of VMware Fusion to be: More Seamless, Safer, More Mac-friendly, with More Tech-Pro Tools. Now is it safer, or did it cross the lines of "The Rules of Virtualization".

In the post, the author proceeds to elaborate on the "3 Rules of Virtualization," similar to Asimov's Three Laws of Robotics. I'll let you read the full article to get the idea, but basically the author is stating that virtualization should not cross "boundaries" between the guest operating systems and the host systems on which the virtualization solution is running.

In the data center, I'd agree with that. Products like VMware Infrastructure, Hyper-V, and XenServer should strictly adhere to the basic properties of virtualization; namely, encapsulation, partitioning, and isolation. Interaction between host and guest are strictly against the rules.

On the consumer side, though, where this article squarely hits---let's be honest, VMware Fusion and Parallels are hardly enterprise data center products---I couldn't disagree more. In fact, the blurring of lines, the removal of boundaries between _virtualized_ and _non-virtualized_ should continue. This is something I first suggested and advocated almost 2 years ago when I [introduced the idea of "application agnosticism"][1]. It was then, when VMware Fusion was still in beta and Parallels was in early release, that I suggested that the blurring of boundaries was **exactly** what needed to occur on the client/consumer side.

Virtualization makes many things possible. The discussions that led up to the introduction of application agnosticism centered around the future of the OS, where some believed that the future of computing revolved around the idea of a collection of VMs---an Internet browsing VM, a security VM that provided anti-virus/firewall/IPS functionality, a third VM for productivity applications, etc. Lots of geeks have setups similar to this on a smaller scale. But ordinary users aren't geeks. They don't understand "virtual machines" and all that jazz. They just want a computer that works. So for virtualization to succeed on the desktop, it has to disappear. It has to fade away into the background, to become unnoticed and invisible. And that's exactly what VMware Fusion seeks to do, and it's what is being added to version 6.5 of VMware Workstation. That's exactly what Microsoft is doing with the technology acquired from Kidaro. They are making virtualization disappear into the background, so the users can focus on what they really want: getting stuff done.

Does the blurring of boundaries create problems? Certainly. So does connecting your computer to the Internet, but I don't see people crying doom and gloom over that! We just install our anti-virus, update our anti-malware, turn on our multiple firewalls, and off we go. Why should it be any different with consumer-grade virtualization? It shouldn't.

On the consumer side, virtualization won't succeed until it becomes transparent. Just like in the recent film _I, Robot_, where the Three Laws of Robotics had to be broken in order to move forward, in the evolution of consumer/client/desktop virtualization the Three Laws of Virtualization are going to need to be broken before we can move forward.

[1]: {{< relref "2006-12-26-application-agnosticism.md" >}}
