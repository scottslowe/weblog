---
author: slowe
categories: Liveblog
comments: true
date: 2009-05-06T13:24:51Z
slug: citrix-synergy-day-2-keynote
tags:
- Apple
- Citrix
- HyperV
- Virtualization
- Xen
title: Citrix Synergy Day 2 Keynote
url: /2009/05/06/citrix-synergy-day-2-keynote/
wordpress_id: 1319
---

The Citrix Synergy Day 2 keynote starts about 8:40 AM with Pat Gelsinger of Intel, a figure becoming increasingly visible at events such as this conference. He starts his talk with a review of IDC statistics on the pressures and forces that are shaping IT, which is much a review of what John Gantz said yesterday.

At least the keynote is a bit more technology focused than [yesterday's keynote][1].

Gelsinger shares that Intel's vision is 100% virtualized, and Intel's engineers and technologists are driven toward eliminating all of the overhead involved in virtualization. That lead into a discussion on the Xeon 5500 processor. Gelsinger then moves into an on-stage demo of a Xeon 5500-based server versus an older server to demonstrate the vast performance improvement that the Xeon 5500 CPUs provide.

Next, Gelsinger moves into a discussion of how Intel is addressing client-side concerns. As with servers, Intel's vision is that every client have some sort of virtualization built-in; this will fulfill what Intel calls the "Dynamic Virtual Client". This is a lead-in to Intel's vPro technology. Continuing Intel's development, they will in 2010 introduce a 32-nm platform that builds upon the technologies found in vPro and enables the Dynamic Virtual Client. What is the Dynamic Virtual Client? It looks surprisingly similar to Citrix's description of how user profiles, applications, operating systems, and hardware are decoupled. The Dynamic Virtual Client falls in the middle between thin clients and "managed rich clients". The Dynamic Virtual Client is a set of hardware that enables any form of end-user experience: application streaming environments, client-side virtual environments, OS image streaming environments, or completely server-hosted environments.

Early customer examples of the Dynamic Virtual Client is Providence Health and a large financial firm (Gelsinger would not disclose the identity of the customer, but the picture said "Stock Exchange"). I'm not so sure that these customer "success stories" have as much to do with Intel and Intel technologies as it is based on technologies like application streaming, OS streaming, and the use of VDI and thin clients.

Gelsinger then introduces Ian Pratt, VP of Advanced Products at Xen.org. Pratt and Gelsinger start discussing the Intel and Citrix collaboration on XenClient, the new name for Project Independence (as announced yesterday). Pratt, of course, would like to see Xen slimmed down far enough to be embedded on the motherboard of every system that comes out of the factory. Pratt moves into a demonstration of a client hypervisor, where the hardware is running two virtual machines: a personal desktop and a professional desktop. In the demonstration, Pratt shows off Windows Vista with the full Aero experience running inside a virtual machine. Pratt then switches to the professional desktop, which is completely isolated and separated from the personal desktop.

This is a very different vision of client virtualization than VMware has shared. In what I've seen of VMware's idea of client virtualization, it's really only about virtualizing a single OS instance---not multiple OS instances. This demonstration---which I assume is a demonstration of XenClient---is, in my opinion, a more powerful, albeit more complex, view of client virtualization.

Naturally, the demonstration wouldn't be complete if they didn't also show off some of the other products and technologies announced here, so Pratt shows off Citrix Dazzle.

With regard to security, Gelsinger and Pratt discuss and explain how XenClient leverages Intel Trusted Execution to guarantee the integrity of the hypervisor and the client OS images, and how it uses Intel VT-d to improve performance. Pratt then demos how XenClient can "punch" applications from one VM into another VM to provide greater security and protection. The demo showed Pratt running an application from the professional desktop seamlessly inside the personal desktop---denoted by a green border around the window---and how that application is protected against potential malware running in the personal desktop. The technology looks a great deal like the Kidaro demonstrations that Microsoft provided last year at Tech-Ed 2008.

XenClient will be available before the end of the year.

With that, Gelsinger ends his portion of the keynote, and Mark Templeton takes the stage again.

Then Pratt comes back on the stage to show off how XenClient running on a MacBook Pro, and then shows Microsoft Outlook running in a seamless window under Mac OS X. This is quite similar to Parallels' Coherence or VMware's Unity mode, but is clearly quite new to the Citrix virtualization crowd. However, it makes me wonder how Apple feels about Citrix virtualizing Mac OS X with a bare metal Type 1 hypervisor. As far as I know, the Mac OS X EULA only permits the virtualization of Mac OS X Server, not the client version.

Templeton then takes center stage again to discuss data centers and clouds. To be effective in providing SaaS---a form of cloud computing according to Templeton---providers must have flexibility, economics, automation, and pay-as-you-go. Templeton compares the economics of "the cloud" by comparing the costs of resources for organizations such as Amazon, Google, YouTube, and Salesforce.com against those same costs for enterprise organizations. (I'm not so sure this is a valid comparison, but that's OK.)

Templeton sees data centers evolving in four steps: consolidated data center, dynamic data center, self-service data center, and cloud-extended data center. The industry is currently in the first step of that evolution. To share some of the announcements around the cloud-extended data center, Templeton introduces Wes Wasson, SVP and Chief Marketing Officer at Citrix.

Wasson jumps right in with some announcements:

* Citrix is announcing a sister product to the NetScaler MPX, the NetScaler VPX product. The NetScaler VPX is a virtual appliance that runs on standard x86 hardware on top of XenServer. This gives organizations the power of the NetScaler MPX as a virtual appliance. To help drive innovation around the NetScaler VPX, Citrix is offering the opportunity to win $10,000 to show the world what's innovative and possible with NetScaler VPX.

* Citrix re-affirms that XenServer 5.5 will continue to be free. The new version adds new guest support, new support for backups, and other key features.

* Citrix is also announcing Citrix Essentials 5.5 for XenServer or Hyper-V. New features include dynamic workload balancing, which sounds like the equivalent of VMware DRS; expanded storage integration via StorageLink and a new "Citrix Ready Open Storage" program; and automated stage management.

Wasson introduces Peter Blum, who performs a demonstration of the new automated stage management functionality found in Citrix Essentials 5.5. Some of this functionality, if I recall correctly, is OEM from another organization (VMlogix, I think?). Blum starts out with a demonstration of the Lab Manager functionality within Citrix Essentials. One nice piece showed off was integration between Dazzle and Lab Manager to provide greater self-service functionality for end-users.

Wasson moves into a couple of cloud computing announcements:

* Citrix Cloud Center (C3) was announced last year (I covered it [here][2]). This week, Citrix is announcing the addition of NetScaler VPX to Citrix C3 to provide flexibility and multi-tenancy.

* Citrix is also announcing the addition of virtual switching technology to XenServer. (Citrix is notoriously quiet about this announcement...odd. No details?)

* Citrix is enabling virutal applications and desktops as a service, and enabling pay-as-you-go functionality in Citrix C3.

* Citrix and Amazon Web Services (AWS) are announcing a partnership and a Citrix C3 Lab that combines Amazon EC2, Amazon S3, Citrix C3, and a series of blueprints that provide deployment guides, configuration notes, architectural overviews, best practices, etc.

Wasson leaves the stage and Templeton returns. Templeton returns to yesterday's vision: transforming data centers into delivery centers, and delivering desktops and applications as a service.

Templeton shows the videos from the Citrix Innovation award finalists---Emory Healthcare, HDFC, and Tesco. After watching the videos, Templeton announces that the winner of the Citrix Innovation award for 2009 is Emory Healthcare.

At this point, the keynote transitions into a customer panel moderated by John Gallant of Network World. I'm wrapping up coverage of the keynote.

[1]: {{< relref "2009-05-05-citrix-synergy-day-1-keynote.md" >}}
[2]: {{< relref "2008-09-15-citrix-announces-citrix-cloud-center.md" >}}
