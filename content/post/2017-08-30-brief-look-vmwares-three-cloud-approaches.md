---
author: slowe
categories: Explanation
comments: true
date: 2017-08-30T09:00:00Z
tags:
- VMworld2017
- VMware
- vSphere
- Cloud
- AWS
title: "A Brief Look at VMware's Three Cloud Approaches"
url: /2017/08/30/brief-look-vmwares-three-cloud-approaches/
---

I'm at VMworld 2017 this week (obviously, based on my tweets and blog posts), and in the general sessions [Monday][xref-1] and [yesterday][xref-2] VMware made a big deal about how VMware is approaching cloud computing and cloud services. However, as I've been talking to other attendees, it's become clear to me that many people don't understand the three-pronged approach VMware is taking.<!--more-->

I should start out by saying that this post hasn't been officially reviewed by VMware (none of my stuff is) and may not align with the "approved" marketing approach, so keep that in mind. This is just me speaking.

As I see it, the three cloud approaches are as follows:

1. Private cloud
2. VMware Cloud on AWS
3. VMware Cloud Services for _native_ cloud workloads

The first option (private cloud) is, I think, pretty much self-explanatory. VMware is offering VMware Cloud Foundation to help streamline some of the infrastructure management in this space, and then the VMware SDDC stack (vSphere, vSAN, and NSX) are layered on top. Couple that with a cloud management platform/automation platform such as OpenStack (VIO would be a good option) or vRealize Automation, and you have a private cloud. (I'm glossing over a few details, but you get the idea.)

The second option (VMware Cloud on AWS, formerly referred to as VMC but not supposed to be called that because it was apparently confusing people) is, I think, also pretty self-explanatory. The technologies behind VMware Cloud Foundation also play in this space (the official name is "VMware Cloud on AWS Powered by VMware Cloud Foundation"), and again it is the VMware SDDC stack (vSphere, vSAN, and NSX) running on top of the bare metal capacity. The key difference is, of course, that it's available on-demand, and the bare metal capacity sits in Amazon's data centers instead of your own. (I'm simplifying a bit, of course.)

The third option is where I think a lot of folks are getting confused. People ask me what I've been focusing on recently and I answer, "I've been working on VMware Cloud Services." They respond, "Oh, VMware Cloud on AWS?" No. VMware Cloud on AWS is _distinct_, _different_ and _distinctly different_. Whereas VMware Cloud on AWS brings the VMware SDDC stack to the public cloud, VMware Cloud Services---also used as an umbrella term to describe any "as a service" offering VMware is providing, which is perhaps where some of the confusion arises---is a set of SaaS offerings designed to work with _native cloud workloads._ NSX Cloud, one of the SaaS offerings in VMware Cloud Services, is designed to bring NSX functionality to _native workloads_ running on AWS (and, later, Azure, as we demonstrated on Monday afternoon---although that was forward-looking and there's no guarantee of when or how it may be delivered to the market). Cost Insight is designed to give you insight into billing and cost usage for _native cloud workloads_ leveraging services like EC2, EBS, and S3 (among others). Network Insight is designed to allow you to have greater visibility into network traffic between and among _native cloud workloads_ running in an AWS VPC.

When I say that to customers and attendees, I can see the "light bulb" suddenly go on, and they realize how powerful this vision is. It's not just about providing consistent infrastructure for private clouds and VMware Cloud on AWS, but it's also about providing consistent operations for VMware-powered clouds (private or VMware Cloud on AWS) **and** native cloud workloads that are not running on a VMware stack. This gives customers a great range of choices in choosing the approach that works best for their organization, their applications, and their industry, and gives them a path forward as those things evolve, change, and grow.

Anyway, I hope this is somewhat helpful.



[xref-1]: {{< relref "2017-08-28-vmworld-2017-day-1-keynote.md" >}}
[xref-2]: {{< relref "2017-08-29-vmworld-2017-day-2-keynote.md" >}}
