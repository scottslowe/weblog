---
author: slowe
categories: Explanation
comments: true
date: 2010-11-01T13:38:23Z
slug: sioc-event-with-emc-mirrorview
tags:
- EMC
- Storage
- Virtualization
- VMware
- vSphere
title: SIOC Event with EMC MirrorView
url: /2010/11/01/sioc-event-with-emc-mirrorview/
wordpress_id: 2155
---

Another topic arose over the last few days on the vSpecialist mailing list around an event that is logged by VMware vCenter Server when you use Storage I/O Control (SIOC) in conjunction with EMC MirrorView. (MirrorView, for those that don't know, is a replication solution for the EMC CLARiiON arrays.) The focus on the discussion was around the fact that vCenter Server logs an event to the effect that an "external I/O workload has been detected on shared datastore running Storage I/O  Control (SIOC) for congestion management". This particular event is documented by VMware in [this VMware KB article](http://kb.vmware.com/kb/1020651).

One of the recommendations for using SIOC is that you not connect "external workloads"--that is, workloads that are not managed by the same vCenter Server instance---to a shared datastore that is enabled for SIOC. I called this out in my liveblog of [this SIOC session at VMworld 2010][1]. Clearly, it's impossible for vCenter Server to enforce limits and shares defined by SIOC when there are workloads external to and not under the control of vCenter Server.

In this particular case, it's entirely OK to ignore this event. Using SIOC in conjunction with MirrorView is OK; the event just stems from the fact that there _is_, in fact, an external workload (MirrorView) that is affecting the datastore. Some vSpecialists suggested that the event shouldn't even be there if we are simply going to tell users to disregard it, and I agree. For now, though, the recommendation is to simply ignore the event and continue.

Longer term, I think it's safe to say that detection of these sorts of conditions, and the reporting of these conditions, will improve. First off, as was pointed out in the discussion thread among the vSpecialists, it's pretty cool that VMware vSphere can detect this. As the integration between the storage layer and VMware vSphere improves (think more detailed instrumentation of the storage layer being made visible to the hypervisor), these sorts of storage awareness will also improve.

Feel free to post any comments, questions, or clarifications below.

**UPDATE:** I mention above that it's OK to ignore this event, but in reality it would be prudent to double-check the reason that this event is being logged. If the only reason this event is being generated is due to replication via MirrorView, then the event is benign and you shouldn't be terribly concerned. However, you might find that there are other factors that are generating this event, in which case you should most definitely take action. Be sure to review the VMware KB article and verify that there are not other contributing factors that might be causing this event.

[1]: {{< relref "2010-09-01-ta8233-prioritizing-storage-resource-allocation-using-storage-io-control.md" >}}
