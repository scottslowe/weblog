---
author: slowe
categories: Information
comments: true
date: 2012-09-10T17:29:59Z
slug: a-vmworld-2012-wrap-up
tags:
- Virtualization
- VMware
- VMworld2012
title: A VMworld 2012 Wrap-Up
url: /2012/09/10/a-vmworld-2012-wrap-up/
wordpress_id: 2825
---

Now that a week or so has passed, I thought I'd post a quick "link review" of VMworld 2012. Think of this like a VMworld-specific version of Technology Short Takes.

## Liveblogging Posts

A number of folks were liveblogging the conference keynotes and conference sessions. As I have in years past, I also posted a few liveblogs where possible. Here are some links:

Rick Scherer: [VMworld 2012 San Francisco - Live Blogging](http://vmwaretips.com/wp/2012/08/27/vmworld-2012-san-francisco-live-blog/)  

Scott Lowe: [VMworld 2012 Keynote, Day 1][1]  

Scott Lowe: [INF-VSP1423, esxtop for Advanced Users][2]  

Scott Lowe: [VMworld 2012 Keynote, Day 2][3]  

Scott Lowe: [APP-CAP2165, Operating Cloud Foundry][4]  

Scott Lowe: [INF-BCO2655, FT for SMP VMs Tech Preview][5]  

Scott Lowe: [APP-CAP2956, Inside the Hadoop Machine][6]  

Maish Saidel-Keesing: [Live Blogging the General Session](http://technodrone.blogspot.com/2012/08/live-blogging-general-session.html)  

Keith Townsend: [VMworld session review - NET2207, VMware vSphere Distributed Switch Technical Deep Dive](http://virtualizedgeek.com/2012/08/30/vmworld-session-review-net2207-vmware-vsphere-distributed-switch-technical-deep-dive/)  

Derek Seaman: [VMworld 2012 - VMware vSphere Hardening to Achieve Regulatory Compliance INF-SEC1840](http://derek858.blogspot.com/2012/08/vmworld-2012-vmware-vsphere-hardening.html)  

Derek Seaman: [VMworld 2012 - Virtualizing SQL Server 2012 APP-BCA1516](http://derek858.blogspot.com/2012/08/vmworld-2012-virtualizing-sql-server.html)  

Derek Seaman: [VMworld 2012 - vSphere HA and Datastore Access Outages INF-BCO2807](http://derek858.blogspot.com/2012/08/vmworld-2012-vsphere-ha-and-datastore.html)  

Derek Seaman: [VMworld 2012 - Securing the Virtual Environment, How to Defend the Enterprise OPS-CSM1209](http://derek858.blogspot.com/2012/08/vmworld-2012-securing-virtual.html)  

Derek Seaman: [VMworld 2012 - vSphere Performance Best Practices INF-VSP1800](http://derek858.blogspot.com/2012/08/vmworld-2012-vsphere-performance-best.html)  

Derek Seaman: [VMworld 2012 - vSphere HA Recommendations INF-BCO2382](http://derek858.blogspot.com/2012/08/vmworld-2012-vsphere-ha-recommendations.html)  

Derek Seaman: [VMworld - Architecting Storage DRS Clusters INF-STO1545](http://derek858.blogspot.com/2012/08/vmworld-architecting-storage-drs.html)  

Derek Seaman: [VMworld 2012 - Insight into vMotion and Futures INF-VSP1549](http://derek858.blogspot.com/2012/08/vmworld-2012-insight-into-vmotion-and.html)  

Derek Seaman had a lot more posts; just check out [this page](http://derek858.blogspot.com/search/label/VMworld%202012) for a full list. Quite impressive, if you ask me. I'm sure there are plenty more that I missed; if I did miss your liveblogging posts, please speak up in the comments below.

## Other Summary Posts

Of course, there were also plenty of other wrap-up/summary posts as well. Here are links to a few:

Erik Sholten: [VMworld 2012 - General Session](http://www.vmguru.nl/wordpress/2012/08/vmworld-2012-general-session/)  

Andre Leibovici: [VMworld EUC1357 Session and Conference Review](http://myvirtualcloud.net/?p=3843)  

Jason Boche: [VMworld 2012 Announcements - Part 1](http://www.boche.net/blog/index.php/2012/08/27/vmworld-2012-announcements-part-i/)  

Cody Bunch: [VMworld Wrap-Up](http://professionalvmware.com/2012/08/vmworld-wrap-up/)  

Eric Sloof: [Wrapping up VMworld 2012](http://www.ntpro.nl/blog/archives/2144-Wrapping-up-VMworld-2012-San-Francisco.html)  

GigaOm: [VMworld shows a VMware in flux](http://gigaom.com/cloud/vmworld-shows-a-vmware-in-flux/)

## Some Vendor Meetings

As many other attendees did, I also had a few vendor meetings around the VMworld timeframe (some were before the show, others at the show). Here are some notes from a few of these.

**Xangati StormTracker:** This product was actually announced ahead of the show, but discussed pretty extensively at the show. Xangati also sponsored a number of the "extra-curricular" activities, such as VMunderground and Spousetivities. (And their event at the speakeasy was very cool.) StormTracker is Xangati's way to leverage their extremely fine-grained metric analysis functionality, giving users the ability to "roll back" to a specific period in time in order to troubleshoot highly transient "storms" across their cloud infrastructures. The product can span multiple vCenter instances and shows correlations between logical objects, like network interfaces, hosts, clusters, datastores. StormTracker employes a heuristic (self-learning) algorithm to identify abnormal behaviors, and also comes with some pre-defined triggers. See another write-up about Xangati StormTracker [here](http://www.vladan.fr/xangati-introduces-stormtracker/).

**Arista:** I also chatted briefly with Arista about the hardware VXLAN tunnel endpoint (VTEP) support they were demonstrating. As many others have pointed out in the comments to [my post on OTV-VXLAN traffic flow][7], the addition of hardware VTEP support (i.e., moving VXLAN into hardware) does indeed change the game significantly. I hope to have time to revisit that analysis soon.

**HyTrust:** I was fortunate enough to get to chat with Eric Chiu of [HyTrust](http://hytrust.com/) as well. Eric and I have been talking since 2009, and I always enjoy talking with him. Unfortunately, most of what he and I discussed isn't stuff I can publicly talk about. If it's any indication, though, I'm pretty stoked about what they are cooking up, and I definitely think they're headed in the right direction. I'd recommend keeping an eye on HyTrust.

## vSphere 5.1 Posts

Naturally, one of the "big items" coming out of VMworld was the announcement of vSphere 5.1, and that garnered a great deal of attention in the online world. Here are a few links that I saw:

Josh Townsend: [vSphere 5.1 Announcement, Features, and Specifications](http://vmtoday.com/2012/08/vsphere-5-1-announcement-features-and-specifications/)  

Alex Muetstege: [VMware releases version 5.1 of vSphere and vCloud Director](http://www.vmguru.nl/wordpress/2012/08/vmware-releases-version-5-1-of-vsphere-and-vcloud-director/)  

Vladan Seget: [Top VMware vSphere 5.1 Features](http://www.vladan.fr/top-features-of-vsphere51/)  

Gabrie van Zanten: [New vSphere 5.1 and vCloud Suite licensing](http://www.gabesvirtualworld.com/new-vsphere-5-1-and-vcloud-suite-licensing/)  

This list is by no means comprehensive; there are **tons** more blog posts out there that are covering the vSphere 5.1 launch, new features, new licensing terms, and all the rest. All the major VMware(-employed) bloggers--[Duncan Epping](http://www.yellow-bricks.com), [Frank Denneman](http://frankdenneman.nl), [Mike Laverick](http://communities.vmware.com/people/Mike_Laverick/blog/), and [William Lam](http://www.virtuallyghetto.com/)--have posts up about some part of vSphere 5.1, so refer to those sites for more in-depth information.

I guess I should wrap things up now; this post has gotten long enough. It was great to see everyone at VMworld 2012, and I'm looking forward to seeing more of my European friends in Barcelona in just a few short weeks. Thanks for reading!

[1]: {{< relref "2012-08-27-vmworld-2012-keynote-day-1.md" >}}
[2]: {{< relref "2012-08-27-inf-vsp1423-esxtop-for-advanced-users.md" >}}
[3]: {{< relref "2012-08-28-vmworld-2012-keynote-day-2.md" >}}
[4]: {{< relref "2012-08-28-app-cap2165-operating-cloud-foundry.md" >}}
[5]: {{< relref "2012-08-28-inf-bco2655-ft-for-smp-vms-tech-preview.md" >}}
[6]: {{< relref "2012-08-30-app-cap2956-inside-the-hadoop-machine.md" >}}
[7]: {{< relref "2011-12-22-otv-and-vxlan-layer-3-connectivity-compared.md" >}}
