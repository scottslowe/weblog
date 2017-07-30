---
author: slowe
categories: Liveblog
comments: true
date: 2012-08-28T10:43:21Z
slug: vmworld-2012-keynote-day-2
tags:
- Virtualization
- VMware
- VMworld2012
title: VMworld 2012 Keynote, Day 2
url: /2012/08/28/vmworld-2012-keynote-day-2/
wordpress_id: 2782
---

This is a live (or reasonably live) blog for the day 2 keynote at VMworld 2012 in San Francisco, CA.

At 8:35 AM, Steve Herrod takes the stage, after joining in with the VMworld 2012 drummers. He sets the stage by indicating that a pretty significant portion of this morning's keynote will be focused on VMware's end-user computing (EUC) vision and the delivery of that vision. Recall that VMware has talked about a number of different components in their EUC vision: mobile phone virtualization, virtual desktop infrastructure, application virtualization, application management, etc. Herrod indicates that today he'll focus on how all these components come together.

This "unification" of VMware's EUC strategy has three components. First, it's about how you can transform your existing legacy applications ("Legacy as a Service"?), how you can broker services, and how you can deliver applications and services to a variety of endpoints.

Herrod starts with the transformation of legacy applications into services, to make them easier to deploy and manage. Naturally, the discussion starts with a look at virtual desktop infrastructure; namely, VMware View (and the 5.1 release in particular). After speaking about the value of View and mentioning several partner-led initiatives (such as the inclusion of VMware View in the Cisco ISR G2 branch router), Herrod transitions into a discussion of Mirage, which came from the Wanova acquisition. Mirage is built on the idea of layers, which allows organizations to "decompose" images into smaller, more manageable chunks, and allows for synchronization of images (and their component layers) across the network. Mirage is considered complementary to View (and I would agree with that assessment), and brings VMware functionality for managing physical desktops and adding functionality for offline computing.

Next up is a "demo" with Vittorio, the fictional employee who shows off various technologies---such as Mirage upgrading him from Windows XP to Windows 7, then migrating his data seamless from his laptop into his View hosted desktop, and subsequently helping him migrate to a virtualized desktop running on Fusion 5 Pro on Mac OS X.

Herrod now switches gears to a discussion on how you can access "traditional" point-and-click applications on a touch-based interface, a la Project AppShift. Daniel Beveridge leads the discussion (and recorded demo) of Project AppShift, which shows off various tasks like switching applications, selecting text, and other tasks. It's quite an interesting technology, but I do wonder how far away VMware is from actually delivering that functionality.

Now the keynote moves away from the application transformation focus to a look at brokering applications and services. Herrod announces the Horizon Suite, as a single, integrated product suite that helps manage and broker desktops, applications, and data between users and devices. Project Octopus is now officially known as Horizon Data. Project AppBlast will be used to deliver desktops to devices (but no official name disclosed just yet). This leads into a demo of some of the technologies within Horizon.

Leading the demo is Ben (Ben Goodman, I think?), who is showing off---for the very first time---the administrative interface for the Horizon suite. In the demo, VMware shows that Horizon will be able to manage and expose XenApp-based applications from within Horizon.

Horizon also embraces the idea of mobile applications as well, through Horizon Mobile (and Herrod alludes to "exciting announcements" coming soon). He announces a Horizon Mobile "container" for iOS that allows VMware Horizon Mobile to deploy and manage applications to iOS devices like the iPhone and iPad. Herrod then turns back to Ben for a demo of the iOS container.

Herrod sums up the discussion on brokering by showing how the Horizon Suite offer benefits to both users as well as IT.

At this point the keynote shifts into the "Partner Challenge," where Diamond partners are given 4 minutes to "wow" the audience with something cool. Cisco does an...interesting...presentation on LISP. Dell shows off some integrations, which---unfortunately---seem based on VMware's previous-generation client technologies. EMC shows off integration between the vSphere Web Client and backup/restore capabilities. HP shows off integration between Matrix infrastructure orchestration and VMware vSphere and vCloud Director. NetApp shows off a couple of features, including non-disruptive data migration. In the audience voting, NetApp wins the competition.

At this point, the keynote concludes.
