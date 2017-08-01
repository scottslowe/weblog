---
author: slowe
categories: Musing
comments: true
date: 2009-03-12T10:28:51Z
slug: this-pretty-much-answers-that-question
tags:
- Cisco
- Networking
- Solaris
- UNIX
- ZFS
title: This Pretty Much Answers That Question
url: /2009/03/12/this-pretty-much-answers-that-question/
wordpress_id: 1224
---

A couple of days ago I posed this question: [is Sun preparing to take on Cisco][1]? The question generated some interesting responses in the comments to the article.

Reader Bill had this to say:

>How on earth would Cisco respond if Sun started introducing products with better performance, at a fraction of the price, built on high volume open source adoption?

As I responded, that's the real $64,000 question, isn't it? That's the premise upon which this entire thing is built---that by using commodity hardware and open source components, Sun can produce high-quality, high-performing network equipment that they can sell for far less than Cisco.

Reader Ed, on the other hand, questioned the validity of this kind of move:

>I would think that partnering with a Juniper or Foundry-type company and OEMing equipment from those companies would be a more prudent move than venturing on their own to create new network devices.

Normally, I would agree with Ed if we were talking about a company that was merely interested in entering a market in order to become a more complete supplier to their customers. That's not Sun's purpose. Sun's purpose is, I think, to fundamentally change the nature of the networking hardware market. How successful they'll be...well, that's another question.

My original article also prompted a response elsewhere on the Internet. Christofer Hoff [thought](http://rationalsecurity.typepad.com/blog/2009/03/sun-vs-cisco-im-getting-my-popcorn.html) my use of the work "distracted" in describing Cisco and Project "California" wasn't appropriate, and in one sense he's correct---"California" is absolutely a natural evolution of Cisco's products and technologies and it does make sense for them. As I pointed out to Hoff, though, being successful with this new solution (I can't call it a server!) will take focus, and while Cisco is focused on "California" Sun has their opportunity.

And it looks like they are [definitely going to take](http://blogs.sun.com/jonathan/entry/commercial_innovation_3_of_4) that opportunity:

>As I've said before, general purpose microprocessors and operating systems are now fast enough to eliminate the need for special purpose devices. That means you can build a router out of a server - notice you cannot build a server out of a router, try as hard as you like. The same applies to storage devices.  
>
>To demonstrate this point, we now build our entire line of storage systems from general purpose server parts, including Solaris and ZFS, our open source file system. This allows us to innovate in software, where others have to build custom silicon or add cost. **We are planning a similar line of networking platforms, based around the silicon and software you can already find in our portfolio.**

The emphasis on that last sentence is mine, just to emphasize the clarity of where Sun is headed. Clearly, it is their intention to leverage OpenSolaris, Crossbow, ZFS, Solaris Zones, etc., to compete directly against Cisco. And Cisco appears to be their primary target, judging from this sentence:

>That means you can build a router out of a server - notice you cannot build a server out of a router, try as hard as you like.

To me, that looks like a direct jab at "California".

So, I guess the question of whether Sun is going to take on Cisco is settled. Hoff, get your popcorn!

[1]: {{< relref "2009-03-09-is-sun-preparing-to-take-on-cisco.md" >}}
