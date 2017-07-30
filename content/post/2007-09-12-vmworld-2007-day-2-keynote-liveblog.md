---
author: slowe
categories: Liveblog
comments: true
date: 2007-09-12T11:10:33Z
slug: vmworld-2007-day-2-keynote-liveblog
tags:
- Virtualization
- VMware
- VMworld2007
title: VMworld 2007 Day 2 Keynote Liveblog
url: /2007/09/12/vmworld-2007-day-2-keynote-liveblog/
wordpress_id: 535
---

Day 2 is here, and along with it is my liveblog. Thanks for reading, and I hope that today's content is useful to you. Feel free to add your thoughts in the comments below!

## Pre-Keynote

I had a great conversation with Mike Laverick of RTFM Education before the keynote started.

Again, about 8:08 AM, the keynote started. This time, it was no professionally produced video with dramatic natural scenes and inspiring music; no, it was a home-produced amateur video about VMware sung by an as-yet-unannounced gentleman. I didn't recognize the song to which he was singing, but the lyrics were actually very well-written. It was very entertaining.

## John Chambers, Chairman and CEO of Cisco Systems

John Chambers starts his keynote out by addressing the pending transition in the markets that are driven by increasingly pervasive connectivity, skyrocketing computing power, and flexible new technologies. However, these "whiz bang" technologies need to be viewed in the priorities of the CEO and CIO. Of course, the priorities of the CEO and the CIO may be different, and those need to be balanced and aligned.

&lt;aside&gt;As an aside, I found it interesting that John moved down from the stage and walked among the crowd in the front. He's definitely a very comfortable and polished speaker.&lt;/aside&gt;

On the importance of catching the transitions---neither being too late nor too early---John discussed Cisco's use of the Internet back in the late 1990s. He predicts a "second phase" of innovation, built on virtualization and harnessing the power of the network (Cisco's network, of course!), creating new services, new support, new sales models, new innovation and acquisition models, and new command/control/collaboration structures. Embracing this next transition will require the CIO to build an infrastructure, in a cost-effective way, that allows CEOs to "catch the transition".

One key factor that distinguishes the first wave of innovation driven by the Internet was the direction of information. In the first phase, information moved from consumers to enterprises and was driven by the enterprises' decisions; in the next phase, information will flow the other way, and will be driven by the decisions of the consumers.

To effectively harness this, to effectively "catch the transition," infrastructures must be safe, open, simple, virtualized, and converged. But all this is just conjecture until John can show us specific, demonstrable products, platforms, or technologies that enable such infrastructures.

It's interesting to me as John continues his keynote how he uses "virtualization" interchangeably with telepresence ("virtual people") and remote collaboration. I suppose that's a valid use of the term, since you are "virtualizing" your meeting members, but it's not a traditional use of the term.

Now John begins to bring his vision of virtualization and its connection to network starts become a bit more clear. It's about end-to-end virtualization (network, server, storage, resource) that enables a network-orchestrated service oriented architecture. (Personally, I'm a bit tired of the overuse of the "SOA" term, but that's just me.)

We finally move away from just talk and into a demo of Cisco's Virtual Data Center. John is joined on stage by Jim, the Chief Demonstration Officer (who comes up with these titles?). In the demo, Cisco shows off a portal that leverages VMware's VirtualCenter API to automatically provision additional resources. In conjunction with this, we see Cisco's VFrame console, which shows us how additional servers running ESX Server were automatically provisioned by VFrame according to a predefined workflow.

Flipping over to VirtualCenter, we see that new ESX Servers and new virtual machines are being provisioned and coming online. Jim then shows the ANS console, which shows physical utilization and shows how physical resources may be reallocated dynamically as needs change. Continuing the demo, we return to the Cisco portal and add 400 users to the order processing system, and then we see how VFrame, VirtualCenter, and ANS are going to accommodate the change in demand created as a result. Like before, additional physical resources are allocated to accommodate the need; and technologies such as VMotion and VMware DRS will distribute the load accordingly.

John wrapped up the keynote with a tie back into business processes and new markets, and how using technologies such as virtualization. It has to not only be a technology architecture, but a business architecture that empowers and enables the business (and the CEO's) vision.

Overall, it was a good keynote, but remarkable for the absence of what most people expected from the keynote: the announcement that Cisco would be shipping third-party switches on ESX Server. I was really looking forward to this kind of announcement. So who's holding up the announcement---Cisco or VMware?

John's keynote was followed by a panel on energy consumption and energy conservation in the data center. I'll blog more on that later on.
