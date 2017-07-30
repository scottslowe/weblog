---
author: slowe
categories: Explanation
comments: true
date: 2010-05-17T15:00:39Z
slug: a-quick-look-at-vplex-and-vmware-srm
tags:
- Cisco
- EMC
- Nexus
- Virtualization
- VMware
- VMwareSRM
- VPLEX
- Storage
title: A Quick Look at VPLEX and VMware SRM
url: /2010/05/17/a-quick-look-at-vplex-and-vmware-srm/
wordpress_id: 1946
---

The announcement of VPLEX last week at EMC World 2010 in Boston has introduced lots of new possibilities in how virtualized data centers should be designed and deployed. Of particular interest to a number of people is how VPLEX interacts with VMware Site Recovery Manager (SRM).

Jason Nash of Varrow mulls over VPLEX and VMware SRM in [this post](http://jasonnash.wordpress.com/2010/05/17/if-i-have-vplex-do-i-need-site-recovery-manager/), where he asks if VMware SRM is even necessary:

>If money was no object, or if you needed VPLEX anyway, where does that leave VMwares Site Recovery Manager?

While many of Jason's points are absolutely valid, there are some other very important things to keep in mind:

1. First, and perhaps most importantly, VPLEX and VMware SRM is not an **"OR"** discussion, it's an **"AND"** discussion. As noted [by Chad Sakac's response](http://jasonnash.wordpress.com/2010/05/17/if-i-have-vplex-do-i-need-site-recovery-manager/#comment-492), there is a VPLEX SRA currently planned. There are some issues to work through---such as the current requirement by VMware SRM for two vCenter Server instances---but I'm confident these issues will be resolved.

2. Second, the behavior of VPLEX in the event of an unplanned outage lends itself well to VMware SRM-like behavior. In the event that the VPLEX cluster in Site A (your primary site) loses connectivity to the VPLEX cluster in Site B (your secondary/DR/failover site), a set of rules defined by the user control which cluster will continue to have read/write access to distributed devices. The other cluster will suspend I/O to distributed devices until a user manually resumes I/O. This makes VPLEX behavior in an unplanned outage act a lot like VMware SRM already, and is probably one of the reasons why a VPLEX SRA is under development. As long as there are steps that can be automated in some programmatic way, there is continued value for VMware SRM.

3. Third, it's important to keep in mind that the requirements for vMotion over distance include spanned Layer 2 VLANs (using something like Overlay Transport Virtualization from Cisco); VMware SRM has no such requirement. Further, VPLEX is currently limited to synchronous distances; VMware SRM is limited only by the underlying replication mechanisms. This means that VMware SRM continues to be a very valid deployment option even in organizations that may also deploy VPLEX.

Jason does make one point that I **absolutely** agree with:

>But, to me, this yet again adds more reasons to virtualize all the applications that you can. Virtualizing the tier 1 apps reduces your DR plan by a large amount with SRM and this would make it even simpler.

The more applications you virtualize, the more you will be able to take advantage of products like VMware SRM and VPLEX. So what are you waiting for? Get virtualizing!
