---
author: slowe
categories: Musing
comments: true
date: 2008-11-07T10:45:11Z
slug: where-is-bluebear-going-with-kodiak
tags:
- Linux
- macOS
- Virtualization
- VMware
- Windows
- Xen
title: Where is Bluebear Going with Kodiak?
url: /2008/11/07/where-is-bluebear-going-with-kodiak/
wordpress_id: 1014
---

I wouldn't go so far as to say that I broke the news on [Kodiak](http://www.bluebearllc.net/kodiak/), but since my first post on Kodiak [back in August][1], Bluebear has seen quite a bit of coverage around the Internet. Fellow virtualization blogger Duncan Epping of [Yellow Bricks](http://www.yellow-bricks.com/) has discussed Kodiak a number of times (here are only a few):

[Bluebear's Kodiak!](http://www.yellow-bricks.com/2008/09/30/bluebears-kodiak/)  

[Bluebear's Kodiak, what's all the fuss about...](http://www.yellow-bricks.com/2008/09/30/bluebears-kodiak-whats-all-the-fuss-about/)  

[Kodiak 0.02 coming out real soon...](http://www.yellow-bricks.com/2008/10/20/kodiak-002-coming-out-real-soon/)

That's not to mention coverage by [virtualization.com](http://virtualization.com/news/2008/07/16/bluebear-koala-kodiak/), [Reuters.com](http://www.reuters.com/article/pressRelease/idUS149341+10-Jul-2008+BW20080710), and numerous other bloggers, experts, and analysts.

But where is Bluebear headed with Kodiak? What is their vision? Well, I don't speak for Bluebear, but I did want to share some insight I'd gathered during a conversation with one of the Kodiak developers. I was curious to know how VMware's announcements of cross-platform vCenter Server and cross-platform VI Client at VMworld 2008 would affect Kodiak. Perhaps because of VMware's market leadership, most people see Kodiak as only a cross-platform VI replacement. The truth is, according to my information, Kodiak's true value lies elsewhere. While it _can_ be viewed as a VI Client replacement, and while it _does_ bring cross-platform functionality to the table, there's more to it than just that. Thus, cross-platform support by VMware---while sorely needed for quite some time---shouldn't really impact Kodiak all that much.

So what is the value of Kodiak beyond cross-platform support? Good question! Here's a couple of points I gathered from of our conversation:

* Multi-hypervisor management: One stated goal for Kodiak has always been to provide the ability to manage multiple, different hypervisors---not only ESX and ESXi, but also Xen, VirtualBox, etc. This is an area that only Microsoft is dabbling in with SCVMM, which will manage Hyper-V and ESX (via VirtualCenter only). Kodiak can manage ESX directly or via VirtualCenter.

* Management via visualization: I don't know if this is what drove Bluebear to use Adobe AIR or if it's a result of using Adobe AIR, but the idea behind managing virtualization with Kodiak is more through visualization than anything else. Bluebear wants users to be able to respond quickly to potential issues by making it possible to see those potential issues instead of waiting for a notification or an e-mail that something's wrong.

I'm sure that Bluebear has all sorts of super-secret stuff in the works that will further differentiate their product from VMware's cross-platform VI Client, even though the two products aren't intended to directly compete.

And, of course, this doesn't take into account Bluebear's hardware side, aka Koala, which doesn't get nearly the same amount of attention as Kodiak. Personally, I'm kinda hoping that the Koala will end up affordable enough for me to pick one up, as I could surely use it to host various virtual servers at home for media streaming, home automation, etc. But I digress...

Anyway, I think I have a pile of beta invites for Kodiak, so if anyone is interested post a comment here and I'll see what I can do. Then you can take a look at the product yourself---keeping in mind that it is a very early beta---and see what you think about the future of Kodiak.

[1]: {{< relref "2008-08-29-bluebear-and-kodiak.md" >}}
