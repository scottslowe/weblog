---
author: slowe
categories: Liveblog
comments: true
date: 2010-08-31T10:53:45Z
slug: vmworld-2010-keynote-day-2
tags:
- Virtualization
- VMware
- VMworld2010
title: VMworld 2010 Keynote, Day 2
url: /2010/08/31/vmworld-2010-keynote-day-2/
wordpress_id: 2059
---

I'm going to try to liveblog the VMworld 2010 keynote this morning. Hopefully I'll be able to keep up with the pace, and hopefully the site won't melt down from additional traffic. Check back regularly and I'll update this post as the keynote progresses.

As usual, the general session is opening with a video. This time, it's a mock documentary discussing "What is the cloud?" The video compares "cloud computing" to pizza. The next reference is to _The Matrix_, where the narrator of the documentary goes to visit the Oracle and is told his mind is a dumb terminal. Pretty funny!

After the video concludes, VMware Chief Marketing Officer Rick Jackson takes the stage. He shares a few interesting statistics: VMworld 2004 was the first conference with about 1400 guests. Last year, there were about 12,500 guests. This year, now in its seventh year, with about 85 countries represented, there are approximately 17,000 attendees this year. Wow---this is a huge increase over last year! Of those, 4,000 new attendees (first time to the VMworld conference). Fifty-five people have attended every single conference; they are the Alumni Elite.

Rick next discussed the hybrid cloud architecture used to power VMworld 2010. The conference uses two data centers on the East Coast along with a private cloud infrastructure here on site.

Next Rick transitions into a discussion of the phases of virtualization. First there's IT Production, and that gives customers cost savings. Next comes business production, where "applications run better virtualized". Rick says that most VMware customers are currently in the business production phase. The third phase is business agility, driven by IT agility and enabled by operational savings and efficiency. This is IT as a Service (ITaaS). Rick stressed the "open" nature of VMware's solutions, harps on VMware's broad hardware support. He announces that OVF (Open Virtualization Format) is now an ANSI standard. He also reminds the attendees that VMware is working on standardizing the vCloud API as an open standard.

Rick next introduces Paul Maritz, who comes out on stage to take over the presentation. Paul spends a few minutes discussing the breadth of VMware's adoption across industries and across geographies. He then transitions into a discussion of the role of the virtualization layer, it's central role in innovation (and being the focus of innovation), it's impact on operations, resource allocation, and the consumption of infrastructure. As he moves into the discussion of virtual data centers, it's pretty clear (to me, at least) where he's headed---he's laying some foundations and defining some terms for a product announcement, and wants to be sure that the audience is at the same place he is in their thought processes.

After a lengthy discussion of the three layers that need innovation---new infrastructure, new application platform, and new end user access---he now moves out of the theoretical into the practical by inviting Steve Herrod, VMware's CTO, out onto the stage.

Steve starts out with a discussion of vSphere and the vSphere 4.1 release. He reviews a few maximums and covers some basic functionality like vMotion, and reminds the audience of increases in the performance of technologies like vMotion (faster individual vMotion migrations and more concurrent vMotion migrations). Steve also discusses the solution to the "noisy neighbor" problem where individual VMs take up too many resources; the fix, of course, is Storage I/O Control and Network I/O Control. He also discusses the vStorage APIs for Array Integration (VAAI). As most readers of this site probably already know, VAAI allows the hypervisor to offload storage operations onto the storage arrays themselves.

Steve Herrod announces the acquisition of Integrien by VMware for their proactive analytics functionality. The product looks quite interesting, but I'm unclear how Integrien will integrate with existing products like AppSense.

Steve moves through a discussion of producers, consumers, their different needs, SLAs, service catalogs (App Stores), "pay as you go" models, and virtual data centers. The focus is on the gap between producers who provision hardware and consumers who request services. And finally, after all the build-up, Herrod announces VMware vCloud Director (aka Project Redwood). VMware sees vCD as the enabling technology that helps address the disconnect between producers and consumers, and enables companies to create virtual data centers.

To help address security in the virtual data center, VMware announces VMware vShield Endpoint, VMware vShield App, and VMware vShield Edge. These products provide offloaded virus protection, hypervisor-level firewalling, and a "traditional" stateful firewall, respectively. It will be interesting to see how these products play with VMware's security partners. Competitor or partner now?

Unfortunately, I have to now leave the General Session to prepare for my 11AM session on EMC Virtual Storage with VMware vSphere. If you're attending, please feel free to tweet (use the hashtag #TA8101) or blog during the session. See you there!
