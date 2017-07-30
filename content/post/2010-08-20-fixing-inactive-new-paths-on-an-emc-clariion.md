---
author: slowe
categories: Explanation
comments: true
date: 2010-08-20T10:00:00Z
slug: fixing-inactive-new-paths-on-an-emc-clariion
tags:
- CLARiiON
- EMC
- FibreChannel
- Storage
title: Fixing Inactive New Paths on an EMC CLARiiON
url: /2010/08/20/fixing-inactive-new-paths-on-an-emc-clariion/
wordpress_id: 2031
---

I ran into an issue in the lab today with some VMware ESX 4.0 hosts and some older CLARiiON CX3 arrays. I'd been working to fix up the lab so that it more properly reflects a "best practices" configuration with dual SAN fabrics, dual-homed HBAs and CNAs, network connections spread across multiple physical switches, etc.---you know, all the wonderful things that we recommend to our customers.

As a result of cross-connecting both the HBAs and the CLARiiON's storage processors to both SAN fabrics, I ended up with more paths to the LUNs than I had previously. This was, of course, fully expected. Upon browsing the properties for the datastore in the vSphere Client, however, I still saw only four paths---two target ports on each storage processor---when I expected to see more. Upon closer inspection, I determined that I wasn't seeing the ports on the array that I had recently connected to the second fabric.

My first thought was that the SAN zoning incorrect. I went back and double-checked the SAN zoning to ensure that all the initiators were, in fact, zoned to see all the targets. OK, so the zoning is correct. Why didn't the extra paths show up?

I double-checked the physical layer; everything was fine there.

That left only the array itself. Logging into Navisphere, I saw in the Connectivity Status window that all the initiators were logged in and registered. (It turns out there is something else in the Connectivity Status window that I should have noticed, but didn't. Read on to find out what I missed.) Hmmm...so that's not it. I manually edited the initiators in the Connectivity Status window so that all the paths were linked to the same host, thinking perhaps that would resolve the problem, but it didn't help. So, thinking that perhaps de-registering and re-registering the initiators might help, I enabled engineering mode in Navisphere so I could do just that. After enabling engineering mode but before I de-registered the initiators, I poked around to see if anything else stuck out at me.

(If you're not familiar with engineering mode on a CLARiiON, just look at the results from [a Google search like this](http://www.google.com/search?hl=en&q=CLARiiON+engineering+mode). It should give you all the information you need.)

While browsing through Navisphere with engineering mode enabled, I noticed something I hadn't noticed before: the VMware ESX host I was troubleshooting was showing up in two different storage groups. This is an error, as a host is only allowed to be in a single storage group at a time. In this case, some of the initiators were showing up in the desired storage group, but two of the initiators were showing up in the ~management storage group. Ah ha! Those initiators were my missing paths. But how to fix it?

It turns out that by looking at the properties of the storage group and then looking at the Hosts tab, there is now (with engineering mode enabled) an Advanced button that allows you to select the specific paths for each host that should be enabled for that storage group. When I opened the Advanced Properties dialog box for the storage group, there's a separate tab for each host in the storage group that lists all the connection paths that should be included. And, sure enough, my two missing paths were there, unchecked! When I checked them and then went back to review the paths from the VMware ESX host, all six expected paths were now present and accounted for.

Now, I'm told that removing the host from the storage group and then re-adding it to the storage group would accomplish the same effect. That, however, is a disruptive process; this method is non-disruptive (as far as I can tell). I'm also told that the Reconnect button---found in the Host Connectivity status window, accessible by right-clicking a specific host and selecting Connectivity Status---will accomplish the same result as well. I can't speak for either of these two options, but I do know that entering engineering mode and enabling all the paths works and works without disruption.

Oh, and remember how I mentioned that there was something I overlooked in the Connectivity Status window? I learned after the fact---after I'd already fixed the problem---that initiators that are blue in the Connectivity Status window are initiators that are not in a storage group. I don't think knowing that up front would have helped all that much, but it's still handy to know.

So there you have it: if you enable new paths from a host to a storage array and the paths don't show up, use engineering mode to ensure that all the paths are enabled for the host in the storage group.

I encourage you to speak up in the comments if you have additional information or other tips/tricks pertaining to this issue. Thanks for reading!

**UPDATE:** A reader has posted in the comments that the Reconnect option will reestablish all paths to the host without disruption. Thanks, Tim!
