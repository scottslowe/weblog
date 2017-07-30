---
author: slowe
categories: Liveblog
comments: true
date: 2013-08-26T12:18:40Z
slug: vmworld-2013-day-1-keynote
tags:
- Virtualization
- VMworld2013
title: VMworld 2013 Day 1 Keynote
url: /2013/08/26/vmworld-2013-day-1-keynote/
wordpress_id: 3255
---

This is a liveblog of the day 1 keynote for VMworld 2013. Depending on network connectivity, this might not get published until after the keynote is finished. The keynote starts, as usual, with a snazzy marketing video.

After the video wraps up, Robin Matlock takes the stage. Robin invites some 10-year veterans of VMworld to share memorable moments from the previous 10 VMworlds.

From there, she launches into a discussion of the "customer virtualization journey"; it's the same 3-step journey of IT production, business production, and IT-as-a-Service that we've heard about for several years. This leads to a talk about VMware's 10 years of innovation, with products and features like ESX, vMotion, DRS, Storage vMotion, and more. Robin lays the foundation for discussing more than just compute virtualization, alluding to innovation in areas other than compute. She then introduces Pat Gelsinger, VMware CEO.

Pat takes the stage and starts talking about the different "eras" of computing: mainframe, client/server, and now the mobile/cloud era. As we are moving into the mobile/cloud era, Pat thinks that four trends are shaping this era: social, mobile, cloud, and big data. Pat believes that it's all about the applications, and that enterprise applications are becoming more like consumer applications. He gives an example of such an app---an app that leverages cloud-based resources to deliver DNA visualizations to mobile devices.

After a brief foray through the potential "role models" for IT professionals, Pat takes us into a discussion of three imperatives: virtualization beyond just compute, automation taking over IT management, and ubiquitous hybrid cloud. He starts discussing these imperatives, and focuses on the expansion of virtualization beyond just compute. This is a software-defined data center discussion---compute virtualization, storage virtualization, network virtualization, and management.

Next up is a customer testimonial from Columbia Sportswear, who talks about how they've embraced the idea of the software-defined data center.

Following the video, Pat launches into a more in-depth discussion of the four pillars of the software-defined data center. Specifically, he starts with a discussion of compute virtualization; in particular, he announces the general availability of vSphere 5.5 and vCloud Suite 5.5. He runs through a brief list of new features: 2x the number of cores, 2x the number of vCPUs, 2x the performance, 32x the size of virtual disks, etc. He pays particular attention to mission-critical apps on VMware, calling out Oracle, SAP, and others. Pat also calls out App-Aware HA and Big Data Extensions as new features that help reinforce the message "Apps love vSphere."

Of course, it's not just about today's apps, but also next-generation apps. Pat announces the availability of Cloud Foundry on vSphere and on vCloud Hybrid Service (vCHS). Cloud Foundry is a well-regarded Platform-as-a-Service (PaaS). This gives rise to the message "Even more apps love vSphere."

This leads Pat into a discussion of VMware's storage-focused features. He says that storage is important, but complex, and must address not only the requirements of today's apps but also the requirements of the next generation of applications. He discusses Storage vMotion, Storage DRS, VASA, and VAAI as milestones along the way to software-defined storage. But what is software-defined storage?

* First, the control plane must be policy-driven.

* Second, the data plane must be virtualized.

* Third, application-specific data services must be virtualized and abstracted from the hardware arrays.

These three components make up software-defined storage. Pat announces VSAN, Virtual SAN, technology that extends the hypervisor to enable software-defined storage. VSAN is in general public beta, and GA is expected in conjunction with the first update to vSphere 5.5 in the first half of 2014. Pat also makes mention of vVols, vSphere Read Flash Cache, and Virsto. However, he still doesn't provide a clear path on how these different technologies will meld together (if they will indeed meld together) in future releases.

Pat reinforces the close partnership that exists between VMware and storage industry partners as the key parts of software-defined storage.

Next, Pat moves into a discussion of extending virtualization to networking. Network virtualization and SDN are the next step to the software-defined data center. He announces VMware NSX, which is VMware's key network virtualization platform. It will support any application, any hypervisor, any cloud management platform, and any physical network hardware. It is the culmination of the merger between VMware's own vCNS and Nicira's NVP technology.

Martin Casado now takes the stage. Martin provides a brief background of what led to his work in networking, and then dives into a more in-depth discussion of network virtualization. He provides an overview of network virtualization, emphasizing the importance of how NSX changes the operational model of the data center. After a brief discussion of the important role of the virtual switch---there are now more virtual switch ports than physical switch ports in data centers---Martin invites three network virtualization customers to join him and Pat on the stage: eBay, Citi, and GE.

eBay discusses how they already have about 3,000 VMs running on VMware's network virtualization software, and no change to the physical infrastructure was required to implement it. Next, Citi---their tagline is "Citi never sleeps"--talks about how they've virtualized over 50% of their server workloads and are now moving into network virtualization. Citi plans to use network virtualization to provide a higher level of security and isolation on a multi-tenant private cloud. Finally, GE talks about network virtualization has been a key part of helping them apply the same technologies and same automation techniques to networking as they've done to compute workloads.

Following the customer discussion, Pat announces partner integrations with VMware NSX. He doesn't go into any specific details, but shows a slide with a whole bunch of logos, and encourages attendees to see the partner integrations in the Solutions Exchange.

Pat now shifts his focus to management and automation. As he discusses cloud management, Pat takes a moment to discuss VMware and OpenStack, and discusses how VMware is "the best choice for OpenStack." That leads to a discussion of hybrid cloud, which in turn leads to a discussion of VMware's vCloud Hybrid Service (vCHS). In particular, Pat calls out common management tools, common networking, common security, and common support. All of this is possible with compromising SLAs, regulatory requirements, or anything else. Pat announces the general availability of vCHS, available to all customers and partners. He refers to it as the "first" true hybrid cloud.

Bill Fathers now takes the stage. Bill takes a moment to introduce himself; he comes from Savvis and has 10 years of experience delivering cloud services. Pat asks him: why join VMware? Bill believes VMware to be "uniquely positioned" to be a leader in delivering hybrid cloud services. Early access to vCHS was announced in June, and according to Bill the team gained very valuable insight as a result of the early access program. Bill starts discussing some customers of vCHS:

* Harley Davidson Dealer Services (HDDS), which used vCHS to deploy a new mobile-friendly app

* The Apollo Group discusses how they expand some of their workloads into vCHS to save money on capital expenditures without having to change the tools they use to manage it

Some of the next new services coming to vCHS include:

* DRaaS (Disaster Recovery as a Service)

* Cloud Foundry (as mentioned by Pat earlier)

* Desktop as a Service (DaaS), based on VMware's Horizon products

Where are the vCHS data centers located? Two new DCs will launch in September: Silicon Valley and Virginia. In October, a new DC will open in Texas. In addition, Savvis will start using vCHS as a delivery method for their own customers. This is a pretty big partnership, in my view.

Pat wraps up with a review of the key components of the software-defined data center, and reinforces the role of the "VMware faithful," the Champions of the Mobile-Cloud Era. And with that, Pat wraps up the keynote.
