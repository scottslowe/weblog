---
author: slowe
categories: News
comments: true
date: 2007-05-10T14:34:12Z
slug: viridian-feature-set-trimmed
tags:
- ESX
- Microsoft
- Virtualization
- VMware
title: Viridian Feature Set Trimmed
url: /2007/05/10/viridian-feature-set-trimmed/
wordpress_id: 451
---

Microsoft has been hyping up its Windows Hypervisor ("Viridian") for quite some time now, talking about how the Viridian feature set is going to leapfrog functionality that is currently available on the market. Now, in [this article](http://news.com.com/Microsoft+cuts+Windows+virtualization+features/2100-1016_3-6182852.html) posted this morning, [Microsoft](http://www.microsoft.com/) has revealed that they have to pull three key features from Viridian in order to meet its delivery schedule.

Of the three features being cut, the most notable to me is live migration. [VMware](http://www.vmware.com/) users will know this better as [VMotion](http://www.vmware.com/products/vi/vc/vmotion.html), the ability to move a live VM from one physical host to another with no downtime. This is a key enabling technology, and the fact that Microsoft won't have this functionality is a _huge_ win for VMware (and, to a lesser extent, [XenSource](http://www.xensource.com/)).

But live migration isn't the only feature on the cutting block:

>The initial release of Viridian also won't support on-the-fly, or "hot," adding of memory, storage, processors or network cards. And it will support computers with a maximum of 16 processing cores---for example, eight dual-core chips or four quad-core chips.

I agree with the article's author that limiting support to 16 cores isn't a huge deal, at least not now. A year from now? Hard to say for sure if it will be an issue then, and if I were VMware or Xen I'd make sure my software didn't have that limit. Even if it doesn't matter, let's face it: marketing is half the battle, and saying that your product can handle more horsepower than Microsoft's product can't hurt.

Likewise, "hot adding" resources isn't a big deal today. The article points out that Xen has this capability today; VMware does not. (As an aside, I'm curious to know if Xen's hot-add functionality is only supported for modified guests. Anyone know?) Note that the article is incorrect in pointing out that hot-add support is mitigated somewhat by live migration; that would be true only for adding resources to the _host_, not the _guest._ Without hot-add support, we would still require downtime at the guest level.

It's not all good for VMware, though. In my humble opinion, VMware needs to stay vigilant and not discount Microsoft (or Xen) as a competitor. If they get complacent, then they will most assuredly lose their leadership position. VMware needs to continue to innovate and drive the market to new areas that were previously considered impossible (such as the experimental Record/Replay support in the newly released [VMware Workstation 6.0](http://www.vmware.com/products/ws/)). What is possible now with Record/Replay? How could this functionality be used in ways that people aren't considering today? Can we continue to add value to ACE and VDI deployments by combining these technologies? It probably would be a good idea for them to release VDM (Virtual Desktop Manager), currently available only through a professional services engagement, to the public as a product as well.

VMware has a tremendous lead in the virtualization market, but smart, nimble, and powerful competitors are waiting for a misstep. Here's hoping there isn't one.
