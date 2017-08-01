---
author: slowe
categories: Liveblog
comments: true
date: 2016-08-30T00:00:00Z
tags:
- VMware
- VMworld2016
- NSX
- VSAN
- Storage
- Networking
- OpenStack
title: VMworld 2016 Day 2 Keynote
url: /2016/08/30/vmworld2016-day-2-keynote/
---

This is a liveblog of the day 2 general session here at VMworld 2016 at Mandalay Bay in Las Vegas, NV. Today, VMware is expected to talk more about containers, end-user computing, and other topics not covered in [yesterday's general session][xref-1] (which focused heavily on Cross-Cloud Services, VMware's new set of SaaS-based offerings for multi-cloud management).

The general session kicks off with Sanjay Poonen taking the stage. Poonen is an energetic speaker who's generally very entertaining and lively. He starts his discussion with a nod to VMware's strong customer loyalty and community, which fosters lifelong learning. That quickly transitions into a discussion of "digital transforamtion"---how technology is affecting many different areas of our lives and our society.

VMware's proposition in digital transformation is two-fold:

1. Transform the data center to make it cloud-ready.
2. Preparing the end-user for the mobile-cloud era.

Poonen re-iterates VMware's vision of "any cloud, any device, any application," focusing primarily on Workspace ONE and the broad ecosystem that has formed/is forming around Workspace ONE. Poonen's discussion of Workspace ONE will focus on three layers:

1. How apps and identity work together; identity management is key.
2. Unified management for desktops and mobile.
3. Wrapping management and security around the entire solution.

To help make this real, Poonen runs through a few live demos of Workspace ONE as it is implemented at VMware. This includes applications like Workday, Boxer (an e-mail application acquired by VMware's EUC team), file repositories (that integrate with Dropbox, Box, OneDrive, and Google Drive), and easy contacts management (including integration with HR systems and Active Directory). Poonen's demos include both iOS devices as well as Android devices.

At this point, Poonen invites Stephanie Buscemi from Salesforce.com to the stage. Buscemi talks briefly about Salesforce One and the broad collection of mobile apps available via Salesforce One, and then moves into a live demo (showing off a sales-focused mobile application).

After Buscemi leaves the stage, Poonen shifts his discussion to desktop users. There's a brief mention of VMware Fusion and VMware Workstation (including mention of free licenses for VMworld app users), but quickly moves on to VMware Horizon. (That's a shame.) VMware AirWatch is up next, followed by a mention of the broad ecosystem of partners in the end-user computing space. Poonen introduces a brief customer video to reinforce his points.

After the video, Poonen moves into a demonstration of unified endpoint management. Again, the demo shows Workspace ONE (using the same interface seen on iOS and Android), and shows how conditional access can be used to prevent posting financial results to Twitter. Poonen also shows how NSX can be integrated into this environment.

Poonen next brings out Orion Hindawi with Tanium to show off TrustPoint to illustrate VMware's efforts around management and security in the EUC context. Hindawi shows off using TrustPoint to examine applications installed across systems based on certain criteria, doing this in near real-time. The demo also showed some forensic analysis capabilities. Poonen jumps in to show off TrustPoint-AirWatch integration as well.

Poonen "passes the baton" to Ray O'Farrell, EVP and CTO of VMware. After setting the stage for the discussion, O'Farrell brings out Kit Colbert, who is CTO for VMware's Cloud-Native Apps Business Unit (CNABU). Colbert talks about how addressing the rise of "modern applications" using technologies like containers means more than addressing the needs of developers---it also means addressing the needs of operators who are running applications in production. The way to address both sides is via enterprise container infrastructure, and VMware offers two approaches: vSphere Integration Containers and Photon Platform.

vSphere Integrated Containers preserves the Docker user experience (users can leverage the same Docker command-line client) while offering operators the set of tools they need to manage and operate applications in production. After talking briefly about VIC, Colbert moves into a demo of VIC. The demo shows [Harbor][link-3] (an open source container registry), VIC, and the container portal that is part of VIC. Colbert also shows how VIC enables operators to use familiar tools like the vSphere Client, VMware NSX, and vRealize Operations to manage, optimize, secure, etc., applications running in containers in production. Colbert also shows using vRealize Automation to automate the provisioning of vSphere Container Hosts (VCHs, part of VIC). Finally, Colbert introduces a video with a set of customer testimonials around VMware's container-centric efforts. vSphere Integrated Containers is open source and [available on GitHub][link-2].

Colbert now transitions into a discussion of Photon Platform, the second thrust that VMware is making into the era of using containers as a basic building block for modern applications. Photon Platform has a key focus on simplicity, integration with popular application frameworks, and a thin layer of API-driven management. Photon Platform is also open source and [available on GitHub][link-1].

O'Farrell takes the stage again to bring a customer focus to the discussion. He brings out Mike Wittig from Nike to share their story of technology transformation using VMware products and solutions. Wittig shares show Nike chose VMware Integrated OpenStack (VIO) and VMware NSX to replatform Nike.com into a new data center last year. Wittig shares that eight billion dollars of revenue is now running on top of this new platform, powered by VIO, vSphere, and VMware NSX.

O'Farrell brings out Rajiv Ramaswami, GM of VMware's NSBU (the team behind NSX). Ramaswami provides a brief overview of his background, and how it's brought him to where he is today with network virtualization. He shares how NSX has seen over 400% growth in the last 18 months, and is now in the "tornado phase" as mentioned by Pat Gelsinger in the day 1 keynote. After a discussion on the vision of NSX, Ramaswami introduces a customer testimonial video, and following the video Ramaswami brings out a customer (sorry, didn't catch the customer name) to demonstrate some VMware NSX functionality. The demo also includes vRealize Network Insight (vRNI, formerly Arkin) to show full visibility and insight into network traffic patterns.

Ramaswami relinquishes the stage to Yanbing Li to talk about hyperconvergence via VSAN. VSAN is seeing tremendous growth and success, with now over 5,000 customers (and adding more constantly). Li talks about VSAN's success, and then introduces a customer testimonial video. After the video, Li shares that VSAN's success is due to the fact that it helps increase operational efficiency, achieve cost savings, and eliminate new IT silos. Li thanks VSAN's ecosystem partners for their support and partnership in driving VSAN adoption and growth.

Li shares that VSAN will be the default storage platform for VIC and Photon Platform, how the VSAN team is driving innovation in analytics and policy-based management, and shares work being done on encryption with VSAN (both in the private cloud and the public cloud). Li demonstrates some of this functionality via a demo before turning the stage back over to O'Farrell to wrap things up.


[link-1]: https://github.com/vmware/photon-controller
[link-2]: https://github.com/vmware/vic
[link-3]: https://github.com/vmware/harbor
[xref-1]: {{< relref "2016-08-29-vmworld2016-day-1-keynote.md" >}}
