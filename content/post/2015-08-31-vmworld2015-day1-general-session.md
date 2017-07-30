---
author: slowe
categories: Liveblog
comments: true
date: 2015-08-31T11:10:00Z
tags:
- VMworld2015
- VMware
- Virtualization
title: VMworld 2015 Day 1 General Session Liveblog
url: /2015/08/31/vmworld2015-day1-general-session/
---

This is a liveblog for the Day 1 general session at VMworld 2015 in San Francisco. For many people, VMworld started yesterday with the Welcome Reception in the Solutions Exchange, but today marks the official kick-off to the event. I'll have to end this liveblog shortly before the general session ends in order to make it to some customer meetings.

The keynote kicks off with a short video about the VMware Cloud Academy, where both "legacy" and "cloud-native" apps can enjoy the Unified Hybrid Cloud. Following that video, Carl Eschenbach takes the stage (along with some "apps"). Eschenbach sets the stage for the session by talking about the momentum and volume of success that VMware has enjoyed (and continues to enjoy). He also calls out VMware's philanthropic efforts, via the VMware Foundation and the #vGiveBack program.

Eschenbach nexts dives a bit deeper on the theme of the show, "Ready for Any." This means VMware technologies and products supporting any application, any cloud, any infrastructure, any time, any place...you get the idea. This theme encompasses SDDC (software-defined data center) initiatives, mobility initiatives, and EUC (end-user computing) initiatives. Eschenbach talks in a a bit more detail about how Unified Hybrid Cloud includes private cloud, managed cloud, or public cloud, and built using a variety of methodologies (build your own, converged infrastructure, or hyperconverged infrastructure). This leads into a short video before Eschenbach brings Mike Benson from DirecTV to the stage.

Benson shares, at a very high level, how DirecTV has been using (and will be using) some of VMware's products and technologies to handle extreme volume spikes (such as that driven by boxing matches, NFL games, etc.). Benson especially called out the use of VMware NSX to help provide greater automation for networking services and for increased security. DirecTV's key initiatives moving forward are cloud, mobility, and big data.

With that, Benson leaves the stage, and Eschenbach provides a "roadmap," if you will, of how the keynotes will proceed over the rest of today and tomorrow. Eschenbach leaves the stage without introducing anyone else, and the session moves into a video talking about how various customers are leveraging hybrid cloud solutions from VMware in a variety of industries and to solve a variety of use cases.

After the video, Bill Fathers takes the stage to talk about public cloud and hybrid cloud. Fathers asks the question, "What is hybrid? Is it as simple as connecting a SaaS application into my existing environment?" Fathers states that it's more than that, and that VMware's vision of the unified hybrid cloud is the right approach to bring together private cloud, managed cloud, and public cloud. Fathers structures his talk around three use cases: disaster recovery, application scaling, and mobile applications.

Disaster recovery is the use case that Fathers starts discussing first, but he doesn't spend a lot of time on that ("you guys have it nailed," he says). Instead, Fathers moves directly to application scaling, and talks about how latency kills an application. Networking, according to Fathers, is the bottleneck. Mobile applications are the next huge challenge. What makes mobile applications challenging? The back-end systems suppporting mobile applications often get deployed in public clouds, but need to connect back to data sources in the private cloud. Fathers also points out that VMware will be delivering a broader and broader collection of application services that will make this platform more and more attractive to developers. Hyrid applications---applications that span both the public cloud and the private cloud---are the future, Fathers states unequivocally. VMware is "ahead of the curve" in being prepared to help customers deal with hybrid applications, and Fathers introduces Raghu Raghuram to help talk about dealing with hybrid applications.

Raghuram provides an overview of why the unified hybrid cloud is the best way to deal with hybrid applications. VMware is making investments along three areas to help further the vision and reach of the unified hybrid cloud. The first area of investment is simplifying the private cloud. The second area of investment is extending into the public cloud. This means extending networking as well as extending management between the private cloud and the public cloud. The third area of investment is increasing the reach of public cloud services and public cloud providers, both from VMware as well as from VMware's partners. Raghuram alludes to some of today's announcements, and brings out Yangbing Li to talk about some of the engineering innovations around the unified hybrid cloud.

Li references announcements around the release of VMware NSX 6.2, VMware Virtual SAN 6.1, and VMware EVO SDDC (and VMware EVO SDDC Manager). VMware EVO SDDC enables zero to software-defined data center in just 2 hours (according to internal VMware testing). That's the major part of the "simplify" portion of the three major areas of investment that Raghuram mentioned earlier (simplify, extend, reach). Li next moves to the "extend" portion, and talks about new developments in this area. The first new development mentioned is the extension of Content Library, first introduced in vSphere 6, to include both private and public clouds. No repackaging needed, Raghuram and Li explain, and they move on to networking (my favorite topic). VMware's solution is the Hybrid Networking Services, built on NSX, that enable features like intelligent routing, strong encryption, WAN acceleration, and encryption (and one other thing I couldn't capture). This leads to a discussion between Raghuram and Li about vMotion, and how you can use vMotion to move a workload between a private cloud and vCloud Air. Naturally, this new functionality in vMotion also enables you to vMotion workloads from vCloud Air to one's private cloud as well. This functionality is being called Cross-Cloud vMotion. These two features---Content Library sync and Cross-Cloud vMotion---are together part of Project Skyscraper.

Li next talks about the use of blueprints to instantiate applications, and how VMware is extending blueprints to include provisioning across both private and public clouds. At this point, Raghuram leaves the stage, and Fathers takes the stage to talk about using all the stuff Li just covered to actually build and deploy a mobile application across private and public clouds.

This leads to a quick demo of Mobile Backend as a Service as provided by a VMware partner named Kinvey.

Li then recaps what she discussed, following the ages-old mantra of "tell people what you're going to tell them, tell them, and then tell them what you told them."

Fathers takes over from Li to summarize what's been shown. I'm going to wrap-up the liveblog at this point, since I have a meeting in just a few minutes and the session is now wrapping up.

**UPDATE**: Sorry about the incorrect title when I first published; I had to run from the Hang Space to a customer meeting and should have done a better job of proofreading.
