---
author: slowe
categories: Information
comments: true
date: 2008-09-16T12:56:22Z
slug: todays-vendor-meetings
tags:
- Security
- Virtualization
- VMware
- VMworld2008
title: Today's Vendor Meetings
url: /2008/09/16/todays-vendor-meetings/
wordpress_id: 902
---

So I met with a number of vendors this afternoon to discuss their product offerings and such, and thought I'd provide a quick summary of the discussions here in case anyone might find them useful.

## VKernel

My first vendor meeting of the day was with [VKernel](http://www.vkernel.com/). I met with Rob Bergin and Christian Simko, and we talked about a wide variety of things, mostly focused around capacity management and VM sprawl. VKernel, as you may already know, offers some virtual appliance-based solutions that help provide visibility into capacity utilization, potential bottlenecks, and capacity trending/forecasting. I haven't had the opportunity to actually use their products yet (and I don't trust vendor demos), but so far I like what I see. They seem to be taking the approach of "do one thing and do it really well", an approach that I myself appreciate. VKernel also recently announced a search-based tool designed to help extract information from VI, something that puts them into competition with the next vendor with whom I spoke.

## Hyper9

Next up was [Hyper9](http://www.hyper9.com/), who recently announced their search-based management product. It's not available yet, but they will soon be opening a private beta and expect to release the product by the end of the year, if I recall correctly. Hyper9 uses the search paradigm to allow administrators to find subsets of virtual machines based on a wide variety of criteria, then see additional information about those VMs or perform actions on those VMs. Hyper9 uses a unique search query syntax but provides various ways of helping to simplify that so the users don't have to learn the syntax. In addition, Hyper9 will offer the idea of workspaces, which are in essence saved queries, that will allow a user to consistently return to a specific subset of systems, like all the hosts whose utilization exceeds 75% or all the VMs lacking this particular patch. I agree with the guys from Hyper9 that the old "tree and folder" approach that has been used in so many applications for so long is getting a bit tired and unwieldy, but I myself am not so sure the general population is ready to ditch it for a search-based interface. Nevertheless, I do find what I saw of the product quite intriguing.

## BlueStripe Software

[BlueStripe Software](http://www.bluestripe.com/page.php?mode=privateview&pageID=1) hails from my neck of the woods, the Raleigh-Durham, NC area. They are a new startup whose product, FactFinder, is focused on providing visibility into application-level performance. The product is actually available (version 1.0 was launched last week), which is a bonus, and a new version (version 1.1) should be available within 30 days or so. The new version was what they showed me, and it's primarily focused on interface clean-up and a few new features. The basic gist of FactFinder is automatically finding relationships between applications (or processes) on different physical and virtual systems. By then gathering information from both sides of the relationship, FactFinder can begin to identify bottlenecks at the application level. It's a crowded market to be in, as everyone and their brother seems interested in bringing more visibility to application performance and identifying relationships. Hopefully, FactFinder will distinguish itself enough from the crowd to be successful.

## Vizioncore

I also met with [Vizioncore](http://www.vizioncore.com/). Although my meeting with them was more focused around the relationship between ePlus and Vizioncore, we did briefly speak about some of the new product announcements that were (then) pending, such as vRanger 4.0, vFoglight, vOptimizer Pro, and vAutomation Suite.

## Tripwire

My last vendor meeting on the day was with Dwayne Melancon of [Tripwire](http://www.tripwire.com/). Tripwire received some great visibility within the VMware community a while back with the release of ConfigCheck, a utility designed to assess the security of VMware ESX according to a predefined set of rules. Tripwire's products can help protect against inadvertent compliance issues, like VMotioning a non-PCI compliant VM onto a PCI-compliant host, for example, and could also help protect against configuration changes that might impact compliance or security.

## RTFM Education

Whilst not a vendor _per se_, I did have a quite lively discussion with Mike Laverick of [RTFM Education](http://www.rtfm-ed.co.uk/) about VMware's VDC-OS announcement, clouds, the future of virtualization, "green" data centers, and cars. I even got a scoop on the [vRainbow announcement](http://www.rtfm-ed.co.uk/?p=597)!
