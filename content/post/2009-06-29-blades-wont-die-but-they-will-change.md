---
author: slowe
categories: Musing
comments: true
date: 2009-06-29T23:54:40Z
slug: blades-wont-die-but-they-will-change
tags:
- Cisco
- FCoE
- Hardware
- Virtualization
- VMware
title: Blades Won't Die, But They Will Change
url: /2009/06/29/blades-wont-die-but-they-will-change/
wordpress_id: 1429
---

I was going through my list of actions in OmniFocus, looking at my projects and actions and evaluating each of them. In my "Potential Posts" project, where I keep links to articles that I might use in a blog post, I found the URL for [this article](http://www.dabcc.com/article.aspx?id=9114) by Steve Kaplan about virtualization, Cisco Nexus, and blade servers. The basic idea of his article is that virtualization and the Cisco Nexus---specifically, the unified fabric---are going to combine to kill blade servers.

I do agree with Steve that there is no innate relationship that means running VMware on blades is somehow "automagically" better:

>It is amazing how frequently we hear IT managers talk about deploying blade servers as an integral component of their new virtual infrastructures - as if there were an obvious synergy between VMware and blade server architectures.

Absolutely! Blades are an option, just like rack mounted servers, and it's up to the customer to choose (or us as consultants to recommend) the form factor that best meets the business needs. It might be blade servers, or it might be rack mounted servers. It just depends. So, on this one point, I agree with Steve.

Yet, at the same time, I also disagree with this point that Steve makes in his article:

>Blade servers have always been an impediment to an optimal virtual infrastructure because they introduce limitations in efficiently utilizing power and cooling resources, budget, flexibility, manageability, bios and firmware updates, performance and troubleshooting.

Here is where Steve and I start to disagree. In fact, this specific article was something of the catalyst for a series of posts, written by colleague and friend Aaron Delp, detailing how blade servers and virtualization work well together:

[Blades and Virtualization Aren't Mutually Exclusive: Part One, HP Power Sizing][1]  
[Blades and Virtualization Aren't Mutually Exclusive: Part Two, IBM Power Sizing][2]  
[Blades and Virtualization Aren't Mutually Exclusive: Part Three, IBM Traditional Expansion Options][3]  
[Blades and Virtualization Aren't Mutually Exclusive: Part Four, HP Traditional Expansion Options][4]

While this series of articles doesn't squarely address all of the arguments against blades and virtualization, the series does make it clear that blades **can** produce power savings vs. rack mounted servers, and that blades **do** offer enough expansion options to accommodate the majority of virtualization deployments.

I also disagree with Steve about the value of the unified fabric, especially considering that right now unified fabric can exist only at the edge of the network and not at the core. That being the case, I find it hard to say that unified fabric is going to kill blade servers. So, again, I have to disagree with Steve's position.

However, Steve's not entirely wrong---virtualization, FCoE and 10Gb Ethernet, and yes even unified fabric _will_ change how blade servers are designed and deployed. Cisco's Unified Computing System (UCS) is one example of how blade servers are going to adapt to these agents of change, and I believe we'll see more examples from other leading vendors in the coming months and years. But will blades die away entirely? No, I don't think so.

Think I'm crazy? Think I'm out of my mind? Feel free to speak up in the comments---courteous comments are always welcome.

[1]: {{< relref "2009-02-04-blades-and-virtualization-arent-mutually-exclusive-part-one-hp-power-sizing.md" >}}
[2]: {{< relref "2009-02-07-blades-and-virtualization-arent-mutually-exclusive-part-two-ibm-power-sizing.md" >}}
[3]: {{< relref "2009-02-13-blades-and-virtualization-arent-mutually-exclusive-part-three-ibm-traditional-expansion-options.md" >}}
[4]: {{< relref "2009-02-16-blades-and-virtualization-arent-mutually-exclusive-part-four-hp-traditional-expansion-options.md" >}}
