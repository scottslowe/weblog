---
author: slowe
categories: Musing
comments: true
date: 2008-12-09T10:07:01Z
slug: continuing-the-fcoe-discussion
tags:
- FibreChannel
- iSCSI
- Standards
- Storage
title: Continuing the FCoE Discussion
url: /2008/12/09/continuing-the-fcoe-discussion/
wordpress_id: 1088
---

A few weeks ago I examined FCoE in the context of it's description as an "I/O virtualization" technology in my discussion of [FCoE versus MR-IOV][1]. (Despite protestations otherwise, I'll continue to maintain that FCoE is **not** an I/O virtualization technology.)

Since that time, I read a few more posts about FCoE in various spots on the Internet:

[Is FCoE a viable option for SMB/Commercial?](http://blog.flickerdown.com/2008/10/14/is-fcoe-a-viable-option-for-smbcommercial/)  

[Is the FCoE Starting Pistol Aimed at iSCSI?](http://blog.fosketts.net/2008/10/16/fcoe-versus-iscsi/)  

[Reality Check: The FCoE Forecast](http://blog.fosketts.net/2008/10/19/fcoe-reality/)

Tonight, after reading a blog post by Dave Graham regarding [FCoE vs. InfiniBand](http://flickerdown.com/?p=349), I started thinking about FCoE again, and I came up with a question I want to ask. I'm not a storage expert, and I don't have decades of experience in the storage arena like many others that write about storage. The question I'm about to ask, then, may just be the uneducated ranting of a fool. If so, you're welcome to enlighten me in the comments.

Here's the question: **how is FCoE any better than iSCSI?**

Now, before your head explodes with unbelief at the horror that anyone could ask that question, let me frame that question with more questions. Note that these are mostly rhetorical questions, but if the underlying concepts behind these questions are incorrect you are, again, welcome to enlighten me in the comments. Here are the framing questions that support my primary question above:

1. FCoE is always mentioned hand-in-hand with 10 Gigabit Ethernet. Can't iSCSI take advantage of 10 Gigabit Ethernet too?

2. FCoE is almost always mentioned in the same breath as "low latency" and "lossless operation". Truth be told, it's not FCoE that's providing that functionality, it's CEE (Converged Enhanced Ethernet). Does that mean that FCoE without CEE would suffer from the same "problems" as iSCSI?

3. If iSCSI was running on a CEE network, wouldn't it exhibit predictable latencies and lossless operation like FCoE?

These questions---and the thoughts behind them---are not necessarily mine alone. In October Stephen Foskett [wrote](http://blog.fosketts.net/2008/10/16/fcoe-versus-iscsi/):

>And iSCSI isn't done evolving. Folks like Mellor, [Chuck Hollis](http://chucksblog.typepad.com/chucks_blog/2008/10/fcoe-gets-taken.html), and [Storagebod](http://storagebod.typepad.com/storagebods_blog/2008/10/netapp-announce-support-for-fcoe.html) are lauding FCoE at 10 gigabit speeds, but seem to forget that iSCSI can run at that speed, too. It can also run on the same CNAs and enterprise switches.

If those Converged Network Adapters (CNAs) and enterprise switches are creating the lossless CEE fabric, then iSCSI benefits as much as FCoE. Dante Malagrino [agrees](http://blogs.cisco.com/datacenter/comments/fcoe_and_iscsi_who_cares_its_all_about_data_center_ethernet/) on the Data Center Networks blog:

>I certainly agree that Data Center Ethernet (if properly implemented) is the real key differentiator and enabler of Unified Fabric, whether we like to build it with iSCSI or FCoE.

Seems to me that all the things that FCoE has going for it--10 Gigabit speeds, lossless operation, low latency operation---are equally applicable to iSCSI as they are functions of CEE and not FCoE itself. So, with that in mind, I bring myself again to the main question: **how is FCoE any better than iSCSI?**

You might read this and say, "Oh, he's an FCoE hater and an iSCSI lover." No, not really; it just doesn't make any sense to me how FCoE is touted as so great and iSCSI is treated like the red-headed stepchild. I have nothing against FCoE---just don't say that it's an enabler of the Unified Fabric. (It's not. CEE is what enables the Unified Fabric.) Don't say that it's an I/O virtualization technology. (It's not. It's just a new transport option for Fibre Channel Protocol.) Don't say that it will solve world hunger or bring about world peace. (It won't, although I wish it would!)

Of course, despite all these facts, it's looking more and more like FCoE is VHS and iSCSI is Betamax. Sometimes the "best" technology doesn't always win...

[1]: {{< relref "2008-11-17-fcoe-versus-mr-iovhuh.md" >}}
