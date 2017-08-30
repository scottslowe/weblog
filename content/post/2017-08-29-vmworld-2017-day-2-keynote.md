---
author: slowe
categories: Liveblog
comments: true
date: 2017-08-29T07:00:00Z
tags:
- VMworld2017
- VMware
- vSphere
- Kubernetes
title: 'Liveblog: VMworld 2017 Day 2 Keynote'
url: /2017/08/29/vmworld-2017-day-2-keynote/
---

This is a liveblog of the day 2 keynote at VMworld 2017 in Las Vegas, NV. Unlike yesterday, I wasn't accosted by the local facilities team trying to get a seat at a table in the bloggers/press/analyst area, so that's an improvement over yesterday. While I'm aware of (most, if not all, of) the announcements that will be made today, I'm still looking forward to the keynote.<!--more-->

Promptly at 9am, Pat Gelsinger takes the stage to kick off the day 2 keynote. He quickly recaps yesterday's announcements and activities, and then rapidly dives into day 2. First up is a "fireside chat" with Michael Dell.

Gelsinger brings Dell onto the stage and they dive into a number of questions submitted by folks.

* Gelsinger fields the first question, which is regarding VMware support. He calls on customers to be sure to let VMware know if support, services, products, etc., don't meet their expectations. Gelsinger refers back to Skyline, which was brushed off yesterday, as a key component of improving VMware's support mechanisms.
* Dell leads off the discussion on a question regarding quantum computing, artificial intelligence (AI) and machine learning (ML), and other forward-looking efforts. Naturally, he positions his company (Dell Technologies) as well-prepared to help customers address these innovations. Gelsinger jumps on to talk about how AI/ML need to be integrated into all technologies, and provide AppDefense as an example.
* When a question about small-medium business (SMB) comes up, Dell starts first to talk about how Dell Technologies is "re-imagining" their products to better address the SMB market. Gelsinger talks for a bit about how VMware is focused and committed to the SMB market as well..
* With regards to ecosystem and VMware's commitment to ecosystem, Dell and Gelsinger both reiterate how important the ecosystem is and how committed they are to preserving and growing the ecosystem. However, maintaining the open ecosystem doesn't mean that Dell and VMware won't continue to pursue deep technical integrations.

At this point, Gelsinger and Dell bring out Rob Mee, CEO of Pivotal, to the stage. Mee starts out by providing a brief background on Pivotal and Cloud Foundry. Mee, Gelsinger, and Dell banter back and forth for a few minutes about Cloud Foundry, but quickly transition into not "what we've done" but instead "what we're doing." Mee points out that Pivotal has deep experience in containers, and references Kubo.

Mee then announces Pivotal Container Service (PKS), launched in partnership with Google Cloud. It offers constant compatibility with Google Container Engine (GKE) and will offer out-of-the-box integration with Google Cloud services. Gelsinger points out that all of the key VMware technologies will integrate seamlessly with PKS. Mee brings out Sam Ramji from Google Cloud.

Ramji join Gelsinger, Dell, and Mee on stage. Ramji talks about the deep experience Google has in running containers (via Borg) and how that led into Kubernetes, and references the tremendous success and momentum that Kubernetes is seeing. Ramji talks about how PKS complements GKE to allow customers to run workloads in their data centers, in the public cloud, or in multiple clouds. The constant compatibility between PKS and GKE is a big plus, and the built-in integration with Google Cloud services (Spanner, BigQuery, etc.) is clearly a win for Google.

Ramji also announces that both VMware and Pivotal are now Platinum members of the Cloud-Native Computing Foundation (CNCF).

Gelsinger wraps up the discussion with Ramji, Dell, and Mee, and transitions to a more technical view on the various announcements. To do that, he brings Ray O'Farrell, CTO of VMware, to the stage.

O'Farrell talks for a few minutes about how and why VMware drives products to market for the benefit of customers. From there, O'Farrell dives into the scenario they've created to help illustrate some of the new technologies that have been announced. While I appreciate the effort that went into creating this scenario, I think I (personally) would've preferred just showing the technologies in action. A quick video about Elastic Sky Pizza (the scenario) sets things up.

First up, O'Farrell brings out Chris Wolf and Purnima Padmanabhan. Padmanabhan starts by reviewing the challenges this fictional company is facing; Wolf brings it "home" to the attendees by talking about how all the companies are facing these sorts of challenges. They then go through a "laundry list" of products like VMware Cloud Foundation, AppDefense, vRealize, etc.

Over the course of the next few minutes, Wolf and Padmanabhan bounce back and forth talking about various products and how those products apply to Elastic Sky Pizza.

The first demo is AppDefense, followed by a quick demo of vRealize Operations, which rapidly leads into a demo of VMware Cloud on AWS. VMware vRealize Network Insight (vRNI) comes up next as they discuss migrating applications from a capacity-bound on-premises data center into VMware Cloud on AWS; in particular, attendees see vRNI helping with migration planning.

VMware vRealize Automation pops up next as the tool used to migrate the application from the on-premises data center to VMware Cloud on AWS.

O'Farrell now returns to the stage to help wrap-up what Wolf and Padmanabhan reviewed showed in their demos.

After another video, Wolf and Padmanabhan talk about the technologies they'll show in the upcoming segment: PKS, a tech preview of a new Automation Service (part of VMware Cloud Services, announced yesterday), and NSX Cloud (demoed live yesterday by yours truly).

In the demo, Wolf shows PKS and walks through the process of deploying a new Kubernetes cluster on vSphere. He then switches to `kubectl` and standard YAML files to deploy pods onto PKS. From there, attendees get to see NSX automatically creating NSX logical objects with _no_ interaction or special effort required from the developers.

Padmanabhan now talks about VMware Cloud Services, a set of SaaS-based offerings to help with cloud-native apps or on-premises environments. These services include NSX Cloud, Wavefront, and others. In this tech preview, she shows an automation service that includes a catalog of blueprints. Reviewing one blueprint, Padmanabhan points out these these blueprints are cloud-agnostic, and support "infrastructure-as-code" by exporting blueprints as YAML files. (Nice!)

Wolf takes the lead again to talk about NSX Cloud, which is (again) part of VMware Cloud Services. Wolf talks about how NSX Cloud (will eventually) span both AWS and Azure, offering customers the ability to take advantage of NSX functionality "as a service." Wolf shows Traceflow functionality between native AWS instances, which is a really cool feature.

O'Farrell comes back to the stage again, once more recapping what attendees have just seen from Wolf and Padmanabhan, and introduces yet another Elastic Sky video.

Following the video, Padmanabhan talks about the technologies we'll see shortly: Wavefront (self-service analytics, very cool stuff) and Workspace ONE Intelligence. She launches quickly into a Wavefront demo, showing how you can quickly and easily drill down through lots of metrics to find exactly what you need to see. From there, Padmanabhan shows off Workspace ONE Intelligence and how it is pulling metrics and telemetry from handheld devices to quickly identify problems in mobile applications.

Wolf takes over to show Workspace ONE Mobile Flows, which are mobile workflows to handle approvals (such as to push out a new version of an application, for example).

O'Farrell returns to the stage, reviewing the demos and discussion from Wolf and Padmanabhan and what was shown/seen there. Is that all there is for Elastic Sky?

After the video concludes, Wolf and Padmanabhan step forward to talk about Internet of Things (IoT), Little IoT Agent (LIOTA), Edge LIOTA, VMware Pulse IoT Center, and Functions-as-a-Service (FaaS)/edge computing. This leads into a demo of some of these technologies, first starting with Pulse IoT Center and then leading into a FaaS demo.

Once Elastic Sky Pizza delivers a pizza, O'Farrell takes the stage again, reviewing the information shared by Wolf and Padmanabhan during their segments and demonstrations.

At this point, O'Farrell wraps up the keynote.
