---
author: slowe
categories: Liveblog
comments: true
date: 2017-11-05T23:30:00Z
tags:
- OpenStack
- Hardware
- Networking
- Storage
title: Issues with OpenStack That Are Not OpenStack Issues
url: /2017/11/05/issues-with-openstack-not-openstack-issues/
---

This is a liveblog of OpenStack Summit session on Monday afternoon titled "Issues with OpenStack that are not OpenStack Issues". The speaker for the session is Sven Michels. The premise of the session, as I understand it, is to discuss issues that arise during OpenStack deployments that aren't actually issues with OpenStack (but instead may be issues with process or culture).<!--more-->

Michels starts with a brief overview of his background, then proceeds to position today's talk as a follow-up (of sorts) to a presentation he did in Boston. At the Boston Summit, Michels discussed choosing an OpenStack distribution for your particular needs; in this talk, Michels will talk about some of the challenges around "DIY" (Do It Yourself) OpenStack---that is, OpenStack that is _not_ based on some commercial distribution/bundle.

Michels discusses that there are typically two approaches to DIY OpenStack:

* The "Donald" approach leverages whatever around, including older hardware.
* The "Scrooge" approach is one in which money is available, which typically means newer hardware.

Each of these approaches has its own challenges. With older hardware, it's possible you'll run into older firmware that may not be supported by Linux, or hardware that no longer works as expected. With new hardware, you may run into issues where Linux doesn't support the new hardware, drivers aren't stable, firmware revisions aren't supported, etc. So, neither approach is perfect.

Continuing the discussion around hardware, Michels points out that older hardware may not have as many CPU cores or offer support for as much RAM in a single box. The ratios of CPUs to RAM for the default flavors may not play well with CPU and RAM support in older hardware platforms, and the default overcommitment settings in OpenStack may also cause some issues with optimizing utilization of the OpenStack cloud. (I'm not sure how this applies to DIY OpenStack, as this would also affect commercial distributions of OpenStack.)

Next, Michels talks about BIOS and BIOS updates, firmware, and IPMI. By a show of hands, Michels shows that a very small portion of attendees actually do BIOS and firmware updates on a regular basis. Michels points out that it may sometimes be necessary to run BIOS, firmware, or IPMI updates in order to optimize performance and manageability. (Again, I'm not sure how this is specific to DIY OpenStack.)

The topic of storage is the next topic that Michels tackles, discussing some design considerations around the use of traditional hard disk drives (HDDs) versus solid state drives (SSDs). He also mentions some considerations around SSD encryption.

Networking support is another area to consider; Michels reminds attendees that support for newer networking technologies like 25G Ethernet may require firmware updates, BIOS updates, kernel updates, and occasional reboots to fix networking outages. Michels points out that you can also run into issues with newer hardware switches and how those switches may (or may not) integrate with or support OpenStack.

Things brings Michels to the topic of operating system updates, and some of the considerations around handling operating system updates. The topic of running your own continuous integration (CI) system is a related topic, and Michels shares some pain points around their own experience trying to use a CI pipeline for OpenStack code in their own deployment.

Related to discussions about operating system updates, Michels spends a few minutes talking about some of the kernel-related issues he's seen. These issues include not only issues with functionality (i.e., something just plain not working) but also issues that result in reduced performance (such as forgetting to tune kernel parameters to better support 25G/40G NICs).

There's also the issue of software issues, although I'm not clear whether Michels is referring to issues with software running on OpenStack or software leveraged by OpenStack but isn't technically part of OpenStack (I tend to lean more toward the latter since he mentioned some issues with Libvirt). Michels points out that this may be a benefit of DIY OpenStack, as many vendor-provided or vendor-packaged OpenStack distributions may not offer you the ability to update specific packages or components.

Michels spends a few minutes talking about some issues with the Puppet manifests/modules provided by the OpenStack Foundation, but there were some issues with these manifests/modules that may require fixes.

Of course, then there's the issue of customers putting your OpenStack cloud to work, and then uncovering potential issues or problems with the implementation. Modifying OpenStack to better accommodate customer demands may result in issues/errors elsewhere in OpenStack. Michels also talks about performance issues with the OpenStack CLI (these issues are apparently still not resolved).

CPU masking by hypervisors running under OpenStack may also affect performance, particularly if features like hardware encryption are masked out. This, in turn, may result in complaints from customers who are using OpenStack instances for use cases where hardware acceleration/offloading would be beneficial.

Michels talks about the need to read the OpenStack documentation; otherwise, you can avoid documented issues with certain configurations (the example Michels provides is running the Nova API in Apache with WSGI, which works [mostly] but isn't supported).

Wrapping up the session, Michels ends the session and opens up for questions from the audience. As I mentioned a couple of times earlier in the liveblog, I don't understand how many of the topics that Michels mentioned are specific to DIY OpenStack; it seems that many of these issues could equally affect commercial packages of OpenStack. I had hoped that Michels' discussion would focus more on process and culture, but there was no discussion of process or culture and instead a strong focus on obscure technical issues. As such, the session was very technical and may have had value for a number of attendees, but I didn't find it as useful as I'd hoped it would be.
