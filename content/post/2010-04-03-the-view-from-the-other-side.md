---
author: slowe
categories: Rant
comments: true
date: 2010-04-03T12:16:50Z
slug: the-view-from-the-other-side
tags:
- UCS
- Virtualization
- VMware
- vSphere
- Hardware
title: The View from the Other Side
url: /2010/04/03/the-view-from-the-other-side/
wordpress_id: 1872
---

Steve Chambers at ViewYonder recently published an article [countering the common "all my eggs in one basket" argument](http://viewyonder.com/2010/03/28/dont-be-a-chicken-cram-your-eggs-into-vsphere-on-ucs/) that is so frequently used as an argument against virtualization. Apparently, a comment in my [Virtualization Short Take #37][1] post led him to believe that I am not, in his words, "fully on board yet". So, I thought it might be helpful to discuss the view from the other side---from the perspective of those who aren't "fully on board yet".

Here's the quote that apparently caught Steve's eye:

>The ages-old discussion of scale up vs. scale out is revisited again in [this blog post](http://itsjustanotherlayer.com/2010/03/scale-up-or-scale-out%e2%84%a2/). I guess the key takeaway for me is the reminder that while VMware HA does restart workloads automatically, there's still an outage. If you're running 50 VMs on a host, you're still going to have an outage across as many as 50 different applications within your organization. That's not a trivial event. I think a lot of people gloss over that detail. VMware HA helps, but it's not the ultimate solution to downtime that people sometimes portray it as.

Steve's article makes some good points; I particularly agree with his assertion that organizations that were bad at IT before virtualization are going to be really bad after virtualization. However, the underlying sentiment of the article seems to be one of "Don't worry, just put all your applications in and everything will be fine". Sorry, I don't agree there. Feel free to put all your eggs in one basket if you prefer, but make sure you understand the value of those eggs and are willing to accept the risks that result.

Let me make something clear: I'm not advocating against high consolidation ratios. What I'm advocating against is a blind race for higher and higher consolidation ratios simply because you can. Steve's article seems to push for higher consolidation ratios simply for the sake of higher consolidation ratios. I'll use a phrase here that I've used with my kids many times: "Just because you _can_ doesn't mean you _should_."

Just because you _can_ run 100 workloads on a single server doesn't mean you _should_ run 100 workloads on a single server. Yes, VMware HA will help you mitigate the overall effect of a hardware outage should one occur, but it doesn't change the fact that an outage will occur. Don't rely upon VMware HA as the "be all end all" solution, because it isn't. Is it good? Yes, it is. Is it good enough for your business when you have very high consolidation ratios? That depends upon your organization and your applications.

I've always favored a measured, deliberate approach to virtualization that takes into consideration all the various aspects of the workloads that you are virtualizing. In my view, it is critical to understand aspects about applications other than disk I/O requirements, CPU utilization, or memory usage. You need to understand aspects like the relationships between applications (which applications are dependent upon other applications?) and business dependencies upon applications (what parts of my business are affected if this application is unavailable?). Understanding the cost of an application outage is another aspect that must be considered in any virtualization approach. In my opinion, the cost of an application outage is the greatest measurement of _impact_.

How much will it cost your business if an application is down for 2 minutes? How much will it cost your business if 50 applications are down for 2 minutes? If these aren't mission-critical applications, then fine; throw them all onto a set of servers and rely on VMware HA to restart them automatically. But do so recognizing that in the event of a hardware or hypervisor failure, there _will_ be an outage. Make sure you understand the impact and the scope of that outage.

If, on the other hand, the cost of having these applications unavailable is too high to allow VMware HA to be the sole mechanism whereby you protect yourself against an outage, then it's time to start employing other mechanisms. Other mechanisms would include VMware Fault Tolerance (FT), OS-level clustering, N+1 redundancy (for those applications that support it; many applications don't), or---here's the kicker---**deliberately suppressing the overall consolidation ratio in order to limit the scope of an outage.** That's right: you might choose to use a lower consolidation ratio for a certain application or group of applications in order to limit the scope of an outage.

This brings us full circle, back to the "scale up vs. scale out" argument that started this whole discussion. Just because an organization _can_ buy a full-width UCS blade with 384GB of RAM and run 100 workloads on it doesn't mean they should. A perfectly valid decision would be to deploy a larger number of smaller servers in a "scale out" fashion so as to limit the scope of an outage. That would go hand-in-hand with the use of other mechanisms like spreading VMware vSphere clusters across multiple chassis to protect against the failure of a single chassis, using VMware FT to protect applications whose downtime cost is too great to rely upon VMware HA alone, or using DRS affinity rules to force workloads onto separate hosts so that the scope of a failure is further constrained. All of these decisions are perfectly valid ways of limiting the scope (and thus the impact) of an outage.

Taken in this context and in this view, you can now see that consolidation ratio is simply another design decision that is made based on both technical and business reasons, and it becomes an aspect of the design that you control, instead of it controlling you. Consolidation ratio becomes just another tool to use in protecting your organization against the impact of a potential outage. Don't shoot for the highest possible consolidation ratio; aim for the right consolidation ratio that meets your organization's needs.

Courteous comments are always welcome!

[1]: {{< relref "2010-03-24-virtualization-short-take-37.md" >}}
