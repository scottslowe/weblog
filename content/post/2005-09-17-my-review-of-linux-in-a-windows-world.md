---
author: slowe
categories: Review
comments: false
date: 2005-09-17T15:33:25Z
slug: my-review-of-linux-in-a-windows-world
tags:
- Windows
- Linux
- Interoperability
- Writing
title: My review of "Linux in a Windows World"
url: /2005/09/17/my-review-of-linux-in-a-windows-world/
wordpress_id: 87
---

A while back, I posted a [blog entry]({{< relref "2005-07-09-new-book-on-linux-windows-integration.md" >}}) mentioning a new [O'Reilly](http://www.oreilly.com/) book titled [_Linux in a Windows World_](http://www.oreilly.com/catalog/linuxwinworld/). While preparing for my trip to Sweden a few weeks ago, I purchased a copy of the book as reading material for the long transatlantic flights.

I still haven't finished reading the entire book, but the parts that I have read have been informative. I've been focusing on the sections regarding centralized authentication, mostly to see if there are things that I need to do to streamline my own centralized authentication scheme (see my post entitled [Linux-AD Integration Direction]({{< relref "2005-07-13-linux-ad-integration-direction.md" >}}) for more information on the approach I took for centralized authentication). Although I didn't find anything earth-shattering, I was able to glean a few helpful details that I will soon be putting into place on my own Linux servers.

I'm looking forward to going over the [Samba](http://www.samba.org/) section of the book, as I've often thought of moving my own file/print services to Linux instead of using [Windows](http://www.microsoft.com/windowsserver2003/default.mspx). I know this sounds strange, but I have to wonder if SMB interoperability would be better between [Mac OS X](http://www.apple.com/macosx/) and Linux instead of between Mac OS X and Windows. Should the opportunity arise for me to test that, I'll post results here.

My only complaint (albeit a very minor one) is that the author tends to gloss over details in some places. That's understandable, to a point, since the intent of the book is not to explain how to implement all the ideas presented. Otherwise, the book would be unmanageable and unwieldy due to the amount of content. To the author's credit, he does point readers to sources of additional information where they can find the details they need for a real-world implementation.

If you are looking for a relatively high-level overview of some of the various ways in which you can improve Linux-Windows integration on your network, you could do much worse than this book. On the flip side, though, be prepared to dig into some other resources when you need additional detail.
