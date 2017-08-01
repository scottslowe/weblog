---
author: slowe
categories: Rant
comments: true
date: 2010-04-26T16:03:39Z
slug: continuing-the-consolidation-discussion
tags:
- Virtualization
- VMware
- VMwareFT
- VMwareHA
- vSphere
title: Continuing the Consolidation Discussion
url: /2010/04/26/continuing-the-consolidation-discussion/
wordpress_id: 1906
---

This is just a quick post inspired by Mike Laverick's recent ["Stupid IT" post](http://searchvirtualdatacentre.techtarget.co.uk/news/column/0,294698,sid203_gci1510732,00.html), in which he weighed in on the blog discussion between Steve Chambers and I regarding "putting all your eggs in one basket," the most common argument against high consolidation ratios and, in some cases, against consolidation in general.

Mike's articles--[part 1](http://searchvirtualdatacentre.techtarget.co.uk/news/column/0,294698,sid203_gci1510732,00.html) and [part 2](http://searchvirtualdatacentre.techtarget.co.uk/news/column/0,294698,sid203_gci1510735,00.html)--are excellent articles. The interesting thing here is that, when you really boil it down, my viewpoint is not that far off from both Steve and Mike (spoiler warning: Mike agrees with Steve). In [my blog post][1], I tried to focus less on whether high consolidation ratios are good or bad but instead to focus on whether the high consolidation ratios---and the impact of the design decision to use high consolidation ratios---will satisfy the needs of the business.

I agree with a number of points from [Steve's post](http://viewyonder.com/2010/03/28/dont-be-a-chicken-cram-your-eggs-into-vsphere-on-ucs/). For example, I agree the root cause of an outage is more likely to be human error than hardware outage. I also agree that building redundancy into the infrastructure helps further reduce the possibility of an outage. Mike makes the same argument:

>The truth is that hardware and software components are so reliable and redundant they hardly ever fail. In fact, so much availability software is geared towards protecting the server from hardware failure that some of my peers are beginning to question why they even buy SKUs that contain VMware HA.

So if everything is so redundant and so stable, why _do_ people buy VMware HA? Why _do_ people use clustering solutions like Windows Failover Clustering? Why _do_ people use VMware FT or Neverfail or any of the rest of it?

The answer is simple: fear. Businesses are afraid of their applications being unavailable. In some cases, this fear is irrational, and from this perspective I agree wholeheartedly with both Mike and Steve: don't use the "all my eggs in one basket" argument with me just because it scares you, just because you're afraid of running all your workloads together.

On the other hand, though, this fear might be justified. What if the application or applications in question are the very lifeblood of the business? If you are an online-only organization, the need for your web site to stay up and accessible is crucial. If the web site is down, lots of money gets lost. In this situation, the fear of being unavailable is justified. It's not irrational---it's based on a keen understanding of the needs of the business and the impact of the outage upon the business. And in those cases, where suppressing consolidation ratios is used deliberately in order to satisfy the needs of the business, I'll accept the "all my eggs in one basket" argument.

Come to me with the "all my eggs in one basket" argument backed by irrational fear and a lack of information, and I'll argue against it every time. Come to me with the "all my eggs in one basket" argument backed by an understanding of how IT aligns with the business and the impact of an outage on the business, and I'll listen to---and possibly even agree with---your position. As I and so many others have stated on numerous occasions, don't pursue high consolidation ratios for the sake of high consolidation ratios. Pursue them because it makes the most sense for the business.

In the end, I guess my point is that both Steve and Mike have missed the point. Not that their viewpoints are irrelevant; quite the opposite! Both of them make very good points that are quite relevant and pertinent to the discussion of "Why not higher consolidation ratios?" Unfortunately, that's not the question that needs to be asked or answered. The question should be, "What is best for the business?" In that context, putting "all your eggs in one basket" isn't always the best answer.

Courteous comments welcome!

[1]: {{< relref "2010-04-03-the-view-from-the-other-side.md" >}}
