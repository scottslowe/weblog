---
author: slowe
categories: Liveblog
comments: true
date: 2016-10-19T00:00:00Z
tags:
- VMware
- VMworld2016
- NSX
- VSAN
- Storage
- Networking
title: VMworld EMEA 2016 Day 2 Keynote
url: /2016/10/19/vmworld-emea-2016-day-2-keynote/
---

This is a liveblog of the day 2 general session at VMworld EMEA 2016 in Barcelona, Spain. I wasn't able to write a liveblog of the day 1 session due to some scheduling/logistical conflicts, but managed to get things arranged for day 2 (well, most of it---I'll have to cut this short so I can get to a customer meeting).

At 9am, Sanjay Poonen takes the stage to kick off the general session. Poonen walks through a number of examples how "digital transformation" is affecting businesses and organizations across a variety of industry verticals. Poonen positions Workplace One as the "Switzerland" solution that bridges different kinds of applications (Windows client-server apps, web apps, and mobile apps) with different kinds of devices (Apple, Google, Samsung, Microsoft). The key ingredients of Workspace One are VDI, EMM, and identity.

Poonen quickly transitions into a demo of Workspace One on an iPhone, showing off how VMware employees use Workspace One to run apps like Workday, Concur, ADP, Boxer (VMware's mobile e-mail client), AirWatch Content Locker, and others. The demo then moves into a demonstration of VDI, including 3-D accelerated graphics, on a Samsung Android tablet. Following the demo, Poonen kicks off a customer testimonial video from Siemens.

After a brief testimonial from Adidas, Poonen moves into the desktop/mobile portion of his keynote. This starts out with a recording of a Skype for Business (S4B) call to demonstrate S4B running in a VDI desktop. Poonen points out how VMware AirWatch is consistently ranked as an industry leader.

The final portion of Poonen's keynote focuses on management. This includes unified endpoint management, especially for Windows 10. Naturally, Poonen points out VMware's technologies can reduce this cost by 15 to 30%. He moves into a demo of unified management of a Windows 10 device, showing off things like conditional access (to prevent the dissemination of information to unauthorized applications or destinations). Poonen's demo includes showing off integrations between AirWatch and NSX as well as integrations between AirWatch and TrustPoint. Clearly, AirWatch is a key component of VMware's strategy and portfolio.

Poonen transfers the stage to Ray O'Farrell, CTO of VMware. Naturally, given O'Farrell's focus on the SDDC, the first topic up is the vSphere 6.5 release. He highlights a few key improvements, from the revamped VCSA to the new HTML5 vSphere Client. After a tour of some of the new features, O'Farrell transitions into a discussion of cloud-native applications, and that leads to a discussion and demo of vSphere Integrated Containers (VIC). Unfortunately, the VIC demos don't include any demonstration of using "standard" Docker tools to work with VIC---a real shame, since that would, in my opinion, really underscore the power of what VMware is doing with VIC. Instead, the demo caters to the more traditional GUI-centric users, who are not the folks who will primarily consume VIC.

To talk about vCloud Air Network, O'Farrell brings out Laurent Allard, CEO of OVH.

Next, O'Farrell brings out Yanbing Li, SVP/GM for Storage and Availability, to talk about VSAN as part of the vSphere 6.5 release. Li discusses improvements in integration, management/automation, and support for an even wider variety of workloads running on VSAN. In particular, Li talks about the support for business critical applications running on VSAN, as well as support for containerized workloads (VIC gets a shout-out). VSAN is also part of Photon Platform. She follows all this up with a brief VSAN 6.5 demo.

At this point, Li brings Raji Ramaswami, EVP/GM of Networking and Security, out onto the stage. Ramaswami reviews the key challenges organizations are facing today (security, speed of provisioning, and application continuity), and talks about NSX helps address those challenges. After talking about a few different customers, Ramaswami launches into a customer video.

Unfortunately, I had to stop the liveblog at this point in order to leave for a customer meeting. It's a shame, because I believe that Kit Colbert was due to take the stage shortly to discuss VMware's cloud-native efforts, including Photon Platform.
