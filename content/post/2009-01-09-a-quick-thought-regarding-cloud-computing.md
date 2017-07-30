---
author: slowe
categories: Rant
comments: true
date: 2009-01-09T13:29:21Z
slug: a-quick-thought-regarding-cloud-computing
tags:
- Standards
- Virtualization
title: A Quick Thought Regarding Cloud Computing
url: /2009/01/09/a-quick-thought-regarding-cloud-computing/
wordpress_id: 1132
---

So, a number of sites have been tossing around [this conversation regarding cloud computing](http://rodos.haywood.org/2009/01/what-is-cloud-conversation.html), [what it is (and isn't)](http://vinf.net/2009/01/08/what-is-the-cloud/), and where it's headed. As I was responding to a private e-mail from another conspirator in this conversation, I came up with something that I thought would be best shared with everyone else.

First, a question: if you've ever architected a large enterprise solution, what was the one thing that gave you the most flexibility in designing that solution?

Think about that for a second.

OK, have an answer? Here's my answer: **standards.**

That's right, standards. How? Let's say you're architecting a large enterprise solution involving servers, storage, software, etc. Now, you may have had your preferences regarding some of the components in that solution---for example, you may have _preferred_ HP ProLiant servers---but the truth of the matter is, you could have just as well used IBM servers or Dell servers. You may have _preferred_ EMC Fibre Channel storage, but the truth of the matter is that you probably could have just dropped in an equivalently-configured HP EVA or an HDS array.

Yes, yes---sometimes there are actual technical requirements around availability, functionality, performance, etc., that drive product selection. We all know that. But in many, many cases, the products we select to create the solutions we recommend are based on preferences.

Why is it that we can mix and match these components together according to our preferences? Because  they all adhere to **standards.** An IBM server will connect to a Gigabit Ethernet switch and communicate across the network in exactly the same fashion as an HP ProLiant or a Dell PowerEdge. These servers run the same x86-based CPUs. A Brocade Fibre Channel switch works in essentially the same fashion as a Cisco MDS. When does this mixing and matching become a problem? When you mix in proprietary extensions, proprietary interconnects, and proprietary protocols.

Ah, proprietary-ness. (Nice word, eh?) That brings us to cloud computing. Where are the protocols that define how clouds, or even cloud components, will communicate with each other? Where are the interconnects that will allow clouds to share data and metadata? Where are the standards that define what that data and metadata are? That's my point: they don't exist. Right now, we are in a world of proprietary interconnects and proprietary protocols that prevent us from mixing and matching cloud computing components or providers. Sure, it can be done, but only through converters and translators and such.

And **that** is my point. When I discuss the lack of definition around cloud computing, it's not because of care how we define cloud computing. It's because there's no structure, no framework, and no standards as to how the cloud is formed. Ironically, perhaps even paradoxically, it's the definitions, the structures, and the frameworks that give the cloud it's flexibility.
