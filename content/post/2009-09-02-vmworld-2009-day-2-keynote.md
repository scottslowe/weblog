---
author: slowe
categories: Liveblog
comments: true
date: 2009-09-02T11:06:25Z
slug: vmworld-2009-day-2-keynote
tags:
- Virtualization
- VMware
- VMworld2009
title: VMworld 2009 Day 2 Keynote
url: /2009/09/02/vmworld-2009-day-2-keynote/
wordpress_id: 1613
---

The day 2 keynote starts with another entertaining video.

After the video concludes, Steve Herrod, VMware's CTO, takes the stage. He echoes that virtualization is a "tectonic shift" in the IT industry. Herrod again shows the three pillars---VMware vSphere, VMware View, and VMware vCenter. Herrod's initial focus is on VMware View, due to the increased emphasis on desktop virtualization and the focus VMware is providing in that arena.

VMware vSphere provides the foundation for desktop virtualization and is the best platform for desktop virtualization due to commonality, security, availability, and efficiency. VMware's advances in this area are coupled with advances by Intel (Xeon 5500) and storage (high-speed block caching, for example). VMware View builds on this foundation to enable faster and more efficient image provisioning, image updating, and policy enforcement. One piece that helps with this is the effort that VMware has placed into decoupling the OS, applications, and user profiles.

VMware is announcing this morning that they will be OEM'ing RTO Virtual Profiles to assist in decoupling user profile/user personality data from the underlying OS image and the applications. Unfortunately, Herrod did not provide any additional detail on exactly how RTO's product will be integrated (or if it will even be integrated).

User experience is another key point. To that end, VMware has been focusing on the user experience to enable the "productive desktop" across the WAN, the "PC-like desktop" across the LAN, and the "rich portable desktop" using VMware CVP (Client Virtualization Platform).

To help with user experience on the LAN and WAN, VMware has been working closely with Teradici on PC over IP (PCoIP). VMware's implementation of PCoIP is software-only, but fully supports hardware acceleration. This enables the use of the same protocol from task workers to knowledge workers to designers. PCoIP will be included in the next version of VMware View and will be shipping later this year.

Employee-owned IT (EOIT) is another area that VMware is trying to enable. This can be accomplished in a couple of different ways. One way is using hosted virtualization with a product like VMware Workstation, VMware ACE, or VMware Fusion. VMware ACE has policy support (and I think that Workstation does as well), but Fusion does not have policy support, so it sounds like VMware is planning on extending policy support across all hosted virtualization platforms.

Of course, CVP is the primary focus for VMware, where they are leveraging their relationship with Intel and using the Intel vPro technology to perform desktop virtualization with a local hypervisor but managed centrally via VMware View. This leads into a demo of CVP. The demo shows Windows 7 running on a local Type 1 hypervisor, and then shows a session using a thin client on a local connection using PCoIP. The demo wraps up with a demonstration of using Wyse PocketCloud on an iPhone.

Regarding VMware's mobile strategy, Herrod begins to discuss the various ways in which VMware is enabling the use of mobile devices. vCenter Mobile Administrator is one product that VMware is providing, plus the VMware View-approved iPhone clients like PocketCloud, and---of course---virtualizing the phones themselves. This leads into a discussion of VMware Mobile Virtualization Platform (MVP).

VMware MVP is about device freedom as well as application freedom. Visa takes the stage and, rightfully so, Herrod asks why Visa is on the stage for mobile virtualization. His focus is about enabling financial services on mobile platforms without compromising security. MVP helps cut complexity to enable that functionality. Herrod then introduces Srinivas Krishnamurthy to show an MVP demo.

The MVP demo primarily focuses on the Visa Mobile app, but Srinivas then exposes the relationship of this demo to MVP: the application he's showing is a Google Android application running on a Windows Mobile phone. This enables users to use whatever application they want regardless of the platform.

Herrod now shifts the focus to VMware vSphere. (He promises not to demonstrate VMware Fault Tolerance again.) Once again Herrod brings up the idea of the software mainframe. VMotion is a key component, a key enabler, of the giant computer (aka the software mainframe aka the cloud). This is the sixth anniversary of VMotion, and VMotion now has maturity, breadth, and automated use. Herrod says VMware estimates a new VMotion migration occurs every 2 seconds. (Where does that calculation come from?)

VMotion was extended to Storage VMotion in late 2006, and with vSphere it was extended again to Network VMotion, and extended again with long distance VMotion via some additional partners and vendors. Herrod sees VMotion as a major part of VMware's drive to greater efficiency (one of three marketing pillars around vSphere: efficiency, control, and choice). This is because VMotion is a foundational technology for VMware DRS. He uses the recent study about DRS and improved performance as an example. VMware DRS is being extended (no timeframe yet) to include I/O. This includes the ability to assign shares and IOPS values to individual VMs.

VMotion and VMware DRS are combined yet again to form a foundation for VMware DPM, which enables greater power efficiency.

Herrod next moves from efficiency to control, and what VMware is doing is this area. His first topic in this space is VMware AppSpeed, which helps organizations provide a level of control over application performance. Next Herrod moves into a discussion of vApp, which is a logical collection of one or more VMs described using OVF. This will help enable the "IT service" policy descriptor. This means embedding SLA definitions into the OVF standard and enabling vSphere to act upon those definitions appropriately.

The VMsafe APIs are another point of control and are now officially available with VMware vSphere (released back in May). Embedding security policies into the OVF description and integrating it with vApp is another method of extending control in a policy-based way.

Next Herrod moves into a demonstration of VMware ConfigControl. This is the first time ConfigControl has been demonstrated. The demo was a bit limited, but considering that it won't be shipping until early next year that's not too terribly surprising.

Herrod finally moves on to the third vSphere marketing tenet: choice. Choice includes choice of hardware and choice of where to run applications. Herrod also sees choice as including self-service IT, which leads to a discussion of Lab Manager. (It sounds to me like Lab Manager is a major central focus point for VMware as enabling self-service IT.)

The last focus point in Herrod's keynote is cloud computing, i.e., VMware vCloud. (I suspect we'll see more information on vCloud Express and the official announcements of the vCloud APIs.) Herrod calls out the use case of Site Recovery Manager to connect two internal data centers as a (limited) form of cloud connectivity that customers are using today.

Long-distance VMotion is another level of cloud connectivity that VMware and associated partners are trying to tackle. There are lots of challenges to be addressed here. Why is long-distance VMotion so important or interesting? It could be follow the sun computing, disaster avoidance, etc. In the partner realm, Cisco is one partner that is working on long-distance VMotion, but there are still network identity challenges. F5 is using BigIP to abstract network identity to help address some of the challenges around long-distance VMotion.

Moving on to the VMware vCloud APIs, Herrod talks about a few applications leveraging the API. He also reiterates that the vCloud API has been submitted to the DMTF for consideration as a multi-vendor standard. This will help foster choice.

At the beginning of the keynote, Herrod laid out three initiatives: View, vSphere, and vCloud. Now he adds a fourth one: vApps. This is more related to the SpringSource acquisition, not to the vApp functionality within vSphere (as far as I can tell). Herrod begins to explain how the SpringSource acquisition allows VMware to move "up the stack" from Infrastructure as a Service (IaaS) into Platform as a Service (PaaS) and provide more products to support Software as a Service (SaaS). Adding SpringSource to the VMware mix gives VMware coverage in 2/3 of the cloud definitions (IaaS and PaaS covered).

This move also allows VMware to optimize the IaaS and PaaS layers to provide even greater performance, mobility, and management for SaaS vendors, developers, and providers. Adrian Coyler, CTO of SpringSource, now takes the stage for a demonstration.

The demonstration shows a Java application being deployed to an external cloud using CloudFoundry, a company that SpringSource themselves acquired just in the last month or so. This demonstration was actually shown yesterday during Maritz' vCloud event.

Herrod wrapped up the keynote with a summary of the key takeaways.

**UPDATE:** Here are some links to additional coverage of the Day 2 keynote:

[VMworld Keynote - Day 2](http://blogs.msdn.com/virtual_pc_guy/archive/2009/09/02/vmworld-keynote-day-2.aspx)  

[Live blogging the VMworld 2009 Day 2 "technical" keynote](http://www.brianmadden.com/blogs/brianmadden/archive/2009/09/02/live-blogging-the-vmworld-2009-day-2-quot-technical-quot-keynote.aspx)  

[Live from VMworld 2009: Day 2](http://www.virtualization.info/2009/09/live-from-vmworld-2009-day-2.html)  

[Thoughts on the VMworld Day 2 Keynote](http://www.chriswolf.com/?p=457)

As with the Day 1 keynote, I also encourage readers to check out the [Planet V12n feed](http://www.vmware.com/vmtn/planet/v12n/) for some great coverage.
