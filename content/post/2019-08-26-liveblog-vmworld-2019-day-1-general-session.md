---
author: slowe
categories: Liveblog
comments: true
date: 2019-08-26T12:00:00Z
tags:
- VMworld2019
- VMware
- Virtualization
- Kubernetes
title: "Liveblog: VMworld 2019 Day 1 General Session"
url: /2019/08/26/liveblog-vmworld-2019-day-1-general-session/
---

This is the liveblog from the day 1 general session at VMworld 2019. This year the event is back at Moscone Center in San Francisco, and VMware has already released some juicy news (see [here][link-1], [here][link-2], and [here][link-3]) in advance of the keynote this morning, foreshadowing what Pat is expected to talk about.<!--more-->

The keynote kicks off with the usual inspirational video, this one incorporating themes and references from a number of high-tech movies, including "The Matrix" and "Inception," among others. As the video concludes, Pat Gelsinger takes the stage promptly at 9am.

Gelsingers speaks briefly of his 7 years at VMware (this is his 8th VMworld), then jumps into the content of his presentation with the theme of this morning's session: "Tech in the Age of Any". Along those lines, Gelsinger talks about the diversity of the VMworld audience, welcomes the attendees in Klingon, and speaks very quickly to the Pivotal and Carbon Black acquisitions that were announced only a few days ago.

Shifting gears, Gelsinger talks about "digital life" and how that translates into millions of applications and billions of devices and billions of users. He talks about how 5G, Edge, and AI are going to further revolutionize digital life, and how that revolution will further affect the technology industry. Coming out of this discussion, Gelsinger waxes philosophical about the nature of technology, and the ability of technology to affect mankind for both good or ill. Gelsinger believes in "tech as a force for good," and believes that VMware and the VMware community bear an enormous responsibility to make that a reality. Gelsinger brings Callum Eade, who swam the English Channel as a fundraising effort to help fight a deadly childhood cancer, as an example of working as a "force for good."

Transitioning, Gelsinger talks about "the law of unintended consequences," and how various technological advances and how they've had unintended and unexpected results. He talks about how VMware works to do "good engineering" and "engineering for good." This leads to a video about Techsoup (a company that helps various nonprofits) and MotherCoders (a nonprofit focused on helping mothers break into the technology industry).

As a result of the impact of technology and the responsibility that technologists have to help ensure technology is used for mankind's good, Gelsinger believes that there is no more important time to be a technologist. He addresses the audience and emphasizes that "you" (the audience) needs to bear this responsibility.

Transitioning yet again, Gelsinger now focuses on talking about VMware's vision, which---in his words---remains unchanged in aiming to deliver any application to any device on any cloud. His discussion leads into a multi-cloud discussion and how VMware is "uniquely" (one of Gelsinger's favorite words) positioned to help companies "build, run, and manage" applications across any cloud.

Starting with the "build" portion of the "build, run, manage," narrative, Gelsinger makes his first mention of Kubernetes. Gelsinger believes Kubernetes will connect the developer and the operator, and he quotes Joe Beda in saying that Kubernetes is "improvisational jazz." With that, Gelsinger brings Joe Beda---now a Principal Engineer at VMware---onto stage.

After some banter between Gelsinger and Beda, Gelsinger makes the official announcement of VMware Tanzu (pronounced "tahn-zoo" in spite of how Gelsinger said it on stage) as a suite of products around the "build, run, manage" narrative. The discussion following the Tanzu announcement also brings in the Bitnami and Pivotal acquisitions and how they will fit into the larger picture within the Tanzu framework.

Immediately on the heels of the Tanzu announcement, Gelsinger and Beda make the Project Pacific announcement. Pacific is all about embedding Kubernetes directly into vSphere. (Make no mistake---having seen the details from the inside of VMware, this isn't a cosmetic integration. This is deep, deep integration.) Beda shares a few details on preliminary performance results showing Pacific running workloads 30% faster than Linux VMs and 8% faster than bare metal environments.

Moving on to the "manage" portion of the "build, run, manage" narrative, Gelsinger and Beda announce VMware Tanzu Mission Control, a cloud-neutral platform for managing Kubernetes clusters no matter where they are. That includes DIY Kubernetes on-premises, DIY Kubernetes on the public cloud, or managed Kubernetes offerings such as AKS, EKS, or GKE. No need to have these clusters running on vSphere; Tanzu Mission Control can manage those clusters where they are today.

Beda leaves the stage, and Gelsinger shifts to talk about CloudHealth. This leads into an announcements around CloudHealth Hybrid, the integration of SecureState into CloudHealth, and enhanced integration with the vRealize suite. Gelsinger takes this opportunity to talk briefly about the differences between multi-cloud (diverse resources and operational models) and hybrid cloud (consistent operational models).

Following on that discussion, Gelsinger talks for a few minutes about VMware Cloud Foundation, and that in turn leads into a discussion about VMware Cloud on AWS (which is built on VMware Cloud Foundation). Topics that Gelsinger touches on include following up on the AWS Outposts announcement, and  expansions of VMware Cloud in AWS into new regions worldwide.

Gelsinger points out that using VMware Cloud on AWS helps customers avoid the costly "replatforming" efforts that are typically seen when modernizing applicatson to run on a public cloud platform. He reiterates that Tanzu will be available on VMware Cloud on AWS, and points to new partnerships such as one with NVidia to enable new types of workloads on VMware Cloud on AWS. To emphasize that last point, Gelsinger shows a video from the CEO of NVidia talking about how the VMware-NVidia partnership enables AI and ML developments.

Gelsinger announces the global availability of VMware Cloud with Microsoft Azure (I'm sure I'm violating some naming/branding stuff here), and also announces the availability of VMware Cloud on Dell EMC. Both are build on VMware Cloud Foundation, and the Dell EMC offering leverages VxRail.

What about hybrid cloud operations? Gelsinger spends a few minutes talking about how VMware is building out a portfolio of products and solutions to help with hybrid cloud operations. He also spends some time discussing solutions for edge and telco. He follows that discussion with a video of Sanjay Poonen talking with various folks from Verizon about the impact of 5G and other technological innovations.

Naturally, a discussion of 5G leads into a discussion on networking, and this results in Gelsinger focusing on VMware NSX and related networking acquisitions such as Velocloud (for SD-WAN) and Avi Networks (now NSX Advanced Load Balancer). Gelsinger next announces NSX Intelligence, a combination of vRealize Network Insight and NSX. It's not clear whether NSX Intelligence leverages other recent acquisitions such as Veriflow.

At this point, Gelsinger brings Sanjay Poonen, VMware's COO, to the stage to lead a discussion with some customers. Poonen is, as usual, a dynamic and energetic speaker. Poonen brings out Rathi Murthy and Tim Snyder from Gap and Freddie Mac, respectively. Poonen makes some small talk with Murthy and Snyder before getting into discussions about how these companies are leveraging technology to achieve their business objectives. Poonen does make sure, of course, to ask Murthy and Snyder about their thoughts on the Tanzu announcements. I do appreciate that Poonen asked both Murthy and Snyder about how VMware can improve, although I'm also quite aware that the discussion was probably heavily scripted.

After Poonen sends Murthy and Snyder off the stage, he launches into a discussion of digital workspace. Unfortunately, I did not capture any of Poonen's announcements around digital workspace (and Workspace ONE, presumably) as I needed to leave the keynote for a meeting.

[link-1]: https://blogs.vmware.com/vsphere/2019/08/introducing-project-pacific.html
[link-2]: https://blogs.vmware.com/vsphere/2019/08/project-pacific-technical-overview.html
[link-3]: https://blogs.vmware.com/cloudnative/2019/08/26/vmware-completes-approach-to-modern-applications/
