---
author: slowe
categories: Liveblog
comments: true
date: 2012-08-27T10:16:34Z
slug: vmworld-2012-keynote-day-1
tags:
- Virtualization
- VMware
- VMworld2012
title: VMworld 2012 Keynote, Day 1
url: /2012/08/27/vmworld-2012-keynote-day-1/
wordpress_id: 2732
---

This is a liveblog from the keynote on Day 1 of VMworld 2012 in San Francisco, CA.

As usual, the keynote starts out with a video playing across the massive screens in Hall D of Moscone North. Michael Dell kicks off the vidoes, followed by the predictably flashy and well-produced EMC segment on transforming IT. Next up are videos by HP and NetApp. Apparently part of the package for a Platinum/Diamond sponsorship is a free video at the top of the day 1 keynote.

At 8:35 PM, the lights went down, the music came up, and the general session officially kicked off with a "recycled percussion"-type show banging on a large "VMworld 2012" set on the stage. This was followed by an...interesting...rap session.

At 8:38, Rick Jackson---Chief Marketing Officer, or CMO, for VMware---takes the stage. Jackson first provides a shout-out to the sponsors, then goes over the schedule for the week with a highlight of the various sessions and events. He then leads into an introduction for Paul Maritz, current CEO of VMware (at least until September 1, when Pat Gelsinger takes over as CEO). At 8:47, Paul Maritz takes the stage.

Maritz takes a look at the last four years, sharing some statistics along the way. For example, in 2008 25% of workloads were virtualized; today, that figure is estimated to be 60%. The number of VCPs increased 5x to ~125K worldwide. The number of attendees at VMworld 2008 was about 13K; this year, the estimate is in excess of 20K attendees. In 2008, everyone was asking, "What is cloud?" Now, according to Maritz, we're asking "How do we build a cloud?" I'd say there's still a little bit of "what is it" out there, but that's just my opinion.

Maritz sees the transformation from desktop-centric/document-centric workflows to stream-centric/mobile and social workflows to be the primary driver over the next 4 years, and he believes this transformation will occur in three broad categories: infrastructure, applications, and access. This is the VMware "three-layer" cake that they've been discussing for quite some time. Nothing really new here so far, but a re-affirmation of what Maritz believes is and will happen in the industry over the next few years.

At this point, Maritz transitions the stage to Pat Gelsinger, incoming CEO for VMware. At 8:57, Pat Gelsinger takes the stage.

Gelsinger lays out VMware's goals and objectives in terms of the percentage of workloads that are virtualized (aiming for >90%) and the amount of time to provision new workloads (shooting for minutes/seconds). All this drives toward the "software-defined data center." Gelsinger transitions from his discussion on the need for the software-defined data center into an introduction of VMware's new vCloud Suite, which will include vSphere, vCloud Director, vCloud Connector, Site Recovery Manager, vCenter Operations, and vFabric Application Director---all rolled into a single SKU to make it easier to buy, deploy, and use. That is followed in rapid succession by the announcement of VMware vSphere 5.1, which (according to Gelsinger) provides the highest performance and proven reliability. And with that, Gelsinger announces that vRAM is "dead" and will no longer be considered a part of VMware's licensing approach.

Gelsinger then moves into a discussion of VMware's moves to address a "multi-cloud" world through their acquisitions of DynamicOps and Nicira. He "solidly" confirms VMware's ongoing participation in and support of the open source and OpenStack communities (as evidenced by the news that VMware is seeking to join the OpenStack Foundation). These moves build on VMware's efforts with Cloud Foundry, a PaaS solution that already runs on multiple platforms. Cloud Foundry appears again in Gelsinger's slides in his discussion on how the applications layer is transforming.

Next up in Gelsinger's discussion are the components of the "access" layer to which Maritz referred earlier. This includes products like View, the Wanova products like Mirage, and the Horizon applications.

At 9:15, Gelsinger shifts his discussion to re-affirm VMware's commitment to partners, and introduces Steve Herrod to the stage. Herrod starts with a deeper look at the vCloud Suite, which Gelsinger introduced and announced earlier. In vSphere 5.1, the "Monster VM" grows to 64 vCPUs, 1 TB of RAM, and 1 million IOPS in a SINGLE VM. That's a pretty sizable workload, but I suspect that's really way beyond what most customers really need.

Next up Herrod introduces a demonstration of Project Serengeti, VMware's efforts to provide additional value to Hadoop running in VMware virtualized environments. It was a nice demo, but clearly a recorded demo (not a live demo). From there he moves to an introduction of Enhanced vMotion, which is VMware's ability to perform live migration without shared storage. (This is really a combination of Storage vMotion and vMotion together.)

Future storage directions include virtual volumes (vVols), virtual flash (making flash a "first class citizen"), and the virtual SAN.

In the networking space, Herrod discusses how VMware is going to address automation and provisioning of networking services. According to Herrod, it starts with the vSphere Distributed Switch, but it takes vCloud Director and other management tools in the vCloud Suite to fully flesh out the story. Apparently a demonstration of that is forthcoming. vXLAN (note the small "v" now) is also a key part of what Herrod believes is necessary to virtualize the network and enable networking in the software-defined data center.

At 9:44, Herrod moves into a discussion of the new vSphere Web Client, and shows some screen shots about seamless movement between the vCloud Director and the vSphere Web Client, and also makes some mention about the extensibility of the vSphere Web Client. Unfortunately, he doesn't mention that all the client-side plugins that people are using today must be completely re-written to work with the new vSphere Web Client. The extensibility theme isn't just for the vSphere Web Client, though; extensibility is also a key part of the vCloud APIs that are provided by vCloud Director in the vCloud Suite.

vCenter Operations Manager gets its moment in the spotlight, of course, as it's included in the vCloud Suite. Also included in the vCloud Suite is vCloud Connector, which provides inter-cloud connectivity between your internal vCloud-powered cloud and vCloud-powered services from external providers as well. This is followed by a demo of vCloud Director and related technologies at 9:51.

At this point, Herrod shifts his focus to the idea of addressing a multi-cloud world (something he blogged about last week, if I recall correctly). There are several components here: Cloud Foundry (an open PaaS that runs on multiple platforms), DynamicOps (for automation and orchestration across multiple clouds), and Nicira (for networking in a multi-cloud environment). Herrod (and Serge, who runs the demo) then shows a demo of Nicira integration into vCloud Director, creating a "cross-platform network" that also automatically populates into Nicira's Network Virtualization Platform (NVP).

Herrod wraps up his session with a look at some interesting technology that combines social networking techniques to manage virtual infrastructure. It's an interesting experiment; I personally don't know how useful or valuable it might actually be. He then closes the session with a discussion of the hands-on labs and he reveals that he IS actually wearing a green VMUG T-shirt under his dress shirt during the keynote.

This now wraps up the keynote for Day 1 of VMworld 2012 in San Francisco.
