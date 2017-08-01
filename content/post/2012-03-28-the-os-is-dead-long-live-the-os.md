---
author: slowe
categories: Musing
comments: true
date: 2012-03-28T17:55:32Z
slug: the-os-is-dead-long-live-the-os
tags:
- HyperV
- Microsoft
- Virtualization
- VMware
- vSphere
- Windows
title: The OS is Dead, Long Live the OS
url: /2012/03/28/the-os-is-dead-long-live-the-os/
wordpress_id: 2567
---

I just finished reading a post on ZDNet titled ["Are Hyper-V and App-V the new Windows Servers?"](http://www.zdnet.com/blog/virtualization/are-hyper-v-and-app-v-the-new-windows-servers/4778) in which the author---Ken Hess---postulates that the rise of virtualization will shape the future of the Microsoft Windows OS such that, in his words:

>The Server OS itself is an application. It's little more than (or hopefully a little less than) Server Core.

The author also advises his readers that they "have to learn a new vocabulary" and that they'll "deploy services and applications as workloads."

Does any of this sound familiar to you?

It should. Almost 6 years ago, I was carrying on a blog conversation (with a web site that is now defunct) about [the future of the OS][1]. I speculated at that point that the general-purpose OS as we then knew it would be gone within 5 to 10 years. It looks like that prediction might be reasonably accurate. (Sadly, I was horribly wrong about Mac OS X, but everyone's allowed to be wrong now and then aren't they?)

It should further sound familiar because almost 5 years ago, Srinivas Krishnamurti of VMware wrote [an article](http://blogs.vmware.com/console/2007/07/get-juiced.html) describing a new (at the time) concept. This new concept was the idea of a carefully trimmed operating system (OS) instance that served as an application container:

>By ripping out the operating system interfaces, functions, and libraries and automatically turning off the unnecessary services that your application does not require, and by tailoring it to the needs of the application, you are now down to a lithe, high performing, secure operating system - Just Enough of the Operating System, that is, or JeOS.

The idea of the server OS as an application container---what Ken suggests in very Microsoft-centric terms in his article---is not a new idea, but it is good to see those outside of the VMware space opening their eyes to the possibilities that a full-blown general purpose OS might not be the best answer anymore. Whether it is Microsoft's technology or VMware's technology that drives this innovation is a topic for another post, but it is pretty clear to me that this innovation _is already_ occurring and will continue to occur.

The OS is dead, long live the OS!

&lt;aside&gt;If this is the case---and I believe that it is---what does this portend for massive OS upgrades such as Windows 8 (and Server 2012)?&lt;/aside&gt;

[1]: {{< relref "2006-12-05-the-future-of-the-os.md" >}}
