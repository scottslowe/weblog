---
author: slowe
categories: Liveblog
comments: true
date: 2008-06-10T09:04:12Z
slug: tech-ed-2008-keynote-liveblog
tags:
- HyperV
- Microsoft
- TechEd2008
- Virtualization
- Windows
title: Tech-Ed 2008 Keynote Liveblog
url: /2008/06/10/tech-ed-2008-keynote-liveblog/
wordpress_id: 726
---

It's Tuesday, June 10, 2008, and I'm in Orlando, Florida, attending Microsoft Tech-Ed 2008. The keynote will be starting in a few minutes. As the keynote progresses, refresh this page to get the latest liveblogged information from the keynote. I'll update this post as often as possible throughout the keynote.

## Pre-Keynote

The conference hall is huge, much larger than the hall used last fall in San Francisco at VMworld 2007 and possibly larger than the massive hall used at VMworld 2006 in Los Angeles. Estimates of attendees, last time I looked, were running around 10,000 people. Fortunately, Microsoft had reserved seats for members of the media at the front of the keynote, so I didn't have any problems finding a seat. Otherwise, I can imagine that I would have been seated somewhere very, very far away.

## Keynote Start

The keynote starts with a performance by a drum entourage. One of the drummers then breaks into dance. It's entertaining.

As the drummers leave the stage, a video starts on the main screens in the hall. The video is a series of testimonials that come together to form the words "Learn", "Connect", and "Explore". As the video ends, Bob Muglia takes the stage.

## Bob Muglia, Senior VP, Server and Tools Business

Bob starts out his speech with a video of Hunter Ely, who used various Microsoft technologies and tools to build a database to track Hurricane Katrina victims in Louisiana. Hunter then takes the stage to talk directly about his experiences. Hunter describes, briefly, how he used Groove and SharePoint to create a collaborative work area to help during the disaster relief efforts---tracking patients, family members, supplies going in and out, etc. Hats off to Hunter for his efforts.

Bob then launches into the Microsoft vision of "Dynamic IT," which is intended to take IT from a cost center to a strategic asset. It's a very common vision for technology companies; I think that I've seen this type of slide dozens of times from dozens of companies. Not that it's a bad idea, it's just not unique or original.

The first major technology area that Bob begins to discuss in greater detail is identity management. Windows Server and Active Directory are described as foundations for identity management. Bob announces the public beta of Identity Lifecycle Manager "2", and brings on-stage Fred Delombaerde to demo the product.

In the demo, Fred shows how Identity Lifecycle Manager "2" can be used to graphically build business processes for user provisioning in Active Directory, Exchange, and other third-party applications. Based on some of the screens and terminology in the demo, it looks as if this is built on top of Microsoft Identity Integration Server. It's looks like a very useful product, but I highly doubt the integration that seemed so easy on-screen will be that easy in reality.

Bob then moves to the topic of interoperability. He speaks briefly about interoperability, finally focusing on the interoperability of distributed applications. This leads into a demo showing interoperability of a .NET application with open source components from a company called WSO2. In the demo, a PHP front-end was swapped out for the original WPF (Windows Presentation Foundation); then a Java order processing system is put in place of a .NET business service. Again, it looks great in the demo, but the reality of such an implementation is far more difficult, I suspect. The PR firm for WSO2 had invited me to attend a repeat of the demo at their booth, but I declined; not being a developer and most of my readers not being developers, it didn't really seem applicable.

We next move to virtualization, where Bob discusses the various types of virtualization, and how these types of virtualization present additional opportunities for Microsoft to enhance interoperability. This leads into a video describing how Kroll Factual Data is using Microsoft virtualization. The benefits that Kroll describes are very typical of any large virtualization implementation; I didn't really see anything specific to Microsoft virtualization technologies. That is, other than "the ability to run virtualization on virtually any platform", which I assume is a knock against VMware's Hardware Compatibility List (HCL) and a plug for Windows' broad device driver support.

Interesting comment: Bob refers to Hyper-V as an "OS component." This leads back to [old discussions][1] regarding whether virtualization should be part of the OS or if it should be separate from the OS.

Regarding Hyper-V performance, Bob indicated that performance tests of Hyper-V have shown that it exceeds the performance of VMware ESX, particularly with regards to I/O performance.

Bob sees Microsoft's various virtualization types as enabling the "dynamic data center." Of course, this is a tried-and-true vision that almost every virtualization vendor has used and plays right into the "Dynamic IT" vision he discussed earlier.

At this point, Rakesh Malhotra comes on the screen to show off using System Center Virtual Machine Manager (SCVMM) to manage Hyper-V, Virtual Server, and VMware ESX. Rakesh launches the VI Client to show that there is a three-node ESX cluster. Flipping over to SCVMM, Rakesh showed that the same ESX cluster shows up side-by-side with a Hyper-V cluster. Rakesh touts Quick Migration as a great new feature of Hyper-V. I won't go back into the whole Quick Migration vs. Live Migration discussion here, but feel free to read [this][2], [this][3], or [this][4] for more information.

I do like the fact that SCVMM 2008, like Exchange 2007, uses PowerShell underneath, and shows you the PowerShell command line to perform an operation after a GUI wizard completes. This allows command-line folks to quickly come up to speed on the matching PowerShell commands.

In the next part of the demo, Rakesh uses SCVMM to perform a VMotion operation on the ESX cluster. Here, at least, Rakesh alludes to the fact that VMotion provides no downtime to the user, whereas Quick Migration does not. Bob chimes in to remind users that live migration will be added to a future version of Hyper-V.

Rakesh then opens up System Center Operations Manager (SCOM) as part of a discussion about how a dynamic data center can take actions on behalf of the organization. This funnels back to SCVMM as Performance and Resource Optimization, or PRO. At first, I thought that this was going to be a comparison against VMware DRS, but this actually goes beyond DRS in that it looks into a VM and makes recommendations about how to satisfy requirements. However, this requires a more significant investment and implementation of the various System Center components. Not only do you need SCVMM, but you also need SCOM as well.

I would suspect that Microsoft's "embrace and extend" approach, bringing ESX into its management umbrella, has a fair amount to do with VMware's [recent acquisition of B-Hive](http://www.virtualization.info/2008/05/vmware-acquires-b-hive.html). That acquisition gives VMware some of the same "application awareness" that Microsoft already possesses via SCOM.

Bob then discusses application virtualization, and in particular discusses the idea of running virtual machines on desktop PCs and using "seamless windows" to blur the distinction between applications running in a virtual machine and applications running locally. (Can anyone say Unity or Coherence?)

Jameel Khalfan comes on stage to demo functionality from Microsoft's [acquisition of Kidaro](http://www.virtualization.info/2008/03/microsoft-acquires-kidaro.html). Jameel shows off some of the Kidaro functionality. I see some similarities to VMware ACE here with regards to the ability to place policies and restrictions, but it goes beyond VMware ACE in that it manages applications and communications across the host and the VM. For example, you can use Kidaro to control copy-and-paste between a VM and the host system. In addition, Kidaro can redirect URLs to a VM. I'll have to be completely honest: this is pretty compelling technology. I haven't seen anything like it from any other major virtualization vendor. If Microsoft can deliver on this type of technology, they will be in a good position to retain their dominance in the desktop space, in my opinion.

The next discussion is the idea of software plus services. David Chow comes on stage to discuss the idea of Active Directory synchronization with online services offered by Microsoft; specifically, Exchange Online, SharePoint Online, and LiveMeeting. This involves the synchronization of the Global Address List (GAL) with Microsoft Online, so that users of Exchange Online can seamlessly share e-mail with non-hosted users, i.e., users running in the on-premise environment. It also involves content migration, like moving the contents of a user's mailbox to the online services. I could see the idea of online services being useful in some environments and for some purposes, and at least Microsoft is making it easier to migrate to that type of environment.

Coming back again to the idea of the dynamic data center, Bob moves into a discussion of managing database storage in the data center. Of course, this focuses on discussions of SQL Server 2008. Bob shows a video from the San Diego Zoo that discusses the role of SQL Server 2008 in the deployment of an application called "iZoofari," that is intended to help zoo patrons more effectively plan their visits, and will allow the zoo to track visitor trends.

Bob announces the first Release Candidate of SQL Server 2008, available immediately. He invites Val Fontama to show off some of the new features in SQL Server 2008. The demo focuses on two key features: data compression and policy-based management. These features are available without any modification to existing database applications.

With that, Bob closed out the keynote. On to my first session!

[1]: {{< relref "2006-12-05-the-future-of-the-os.md" >}}
[2]: {{< relref "2007-07-23-live-migration-vs-quick-migration.md" >}}
[3]: {{< relref "2008-04-03-quick-migration-revisited.md" >}}
[4]: {{< relref "2008-04-25-my-thoughts-on-the-live-migration-quick-migration-discussion.md" >}}
