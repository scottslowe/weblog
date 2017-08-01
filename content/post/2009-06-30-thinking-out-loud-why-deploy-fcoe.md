---
author: slowe
categories: Musing
comments: true
date: 2009-06-30T10:01:24Z
slug: thinking-out-loud-why-deploy-fcoe
tags:
- Cisco
- FCoE
- Networking
- Storage
- ToL
title: 'Thinking Out Loud: Why Deploy FCoE?'
url: /2009/06/30/thinking-out-loud-why-deploy-fcoe/
wordpress_id: 1431
---

This is another one of my "thinking out loud" posts. This time, the question I'm mulling is this one: why deploy FCoE?

I haven't hid the fact that I'm not really a fan of FCoE (see [here][1] or [here][2]), but I was starting to warm to the technology and thought that I was beginning to see some benefits to deploying FCoE. Namely, the fact that FCoE is inherently very compatible with "traditional" FCP, allowing organizations to leverage their existing FCP installation while transitioning to FCoE. Some hands-on time I'd recently spent with a Cisco Nexus 5000 switch showed me just how closely aligned the two technologies are and how (relatively) easy it was to extend an FC fabric using FCoE. OK, I think I get this.

Then, a few days ago, I read this article [on FCoE divergence](http://www.theregister.co.uk/2009/06/25/fcoe_divergence/). Given that The Register can sometimes be quite sensationalist (and that's putting it mildly), I contacted a colleague of mine whose input and knowledge I trust. He informed me that FCoE was currently limited in that FCoE is not multi-hop enabled---meaning, you can't connect FCoE initiators on one switch to FCoE targets on another switch. (Apparently, this shortcoming is due to be corrected shortly.)

_Whoa!_ That's a limitation of which I was not aware. And with that limitation in mind, knowing that FCoE will---for the time being at least---be limited to convergence at the edge, I have to ask: _why deploy FCoE at all?_ What real and specific benefits does an organization seek to gain by deploying FCoE as opposed to just deploying FC? Is the edge convergence really that worthwhile and valuable?

[1]: {{< relref "2008-12-09-continuing-the-fcoe-discussion.md" >}}
[2]: {{< relref "2009-02-20-is-unified-fabric-an-inevitability.md" >}}
