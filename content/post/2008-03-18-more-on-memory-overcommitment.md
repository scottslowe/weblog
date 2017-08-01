---
author: slowe
categories: Rant
comments: true
date: 2008-03-18T23:01:42Z
slug: more-on-memory-overcommitment
tags:
- Citrix
- HyperV
- Microsoft
- Virtualization
- VMware
- Xen
title: More on Memory Overcommitment
url: /2008/03/18/more-on-memory-overcommitment/
wordpress_id: 664
---

After a brief mention of this topic in [Virtualization Short Take #4][1], the battle between Citrix, Microsoft, and VMware over memory overcommitment has heated up.

The latest round comes from VMware, who provides some [real-world statistics on memory overcommitment](http://blogs.vmware.com/virtualreality/2008/03/memory-overcomm.html). In addition, I'll draw readers' attention to [this comment](http://blogs.vmware.com/virtualreality/2008/03/cheap-hyperviso.html#comment-107071284) on VMware's [original article](http://blogs.vmware.com/virtualreality/2008/03/cheap-hyperviso.html), in which a VMware customer describes the benefits his organization is seeing from memory overcommitment. (BTW, this commenter apparently also started a VMware Communities thread which was, in turn, the basis for [this article](http://www.yellow-bricks.com/2008/03/18/virtualizing-citrix/) by Duncan over at Yellow Bricks. My, what a tangled web we weave!)

In any case, VMware's response uses real data from a customer; only the names have been changed to protect the innocent. In the case study, a 64GB server has been oversubscribed to support VMs requiring 89GB of RAM, and only 20GB of the server's 64GB is actually in use. So, by reducing the RAM configured in the server, VMware comes up with a way to show that---in this very specific example, at least---it is cheaper to buy VMware than to add RAM to the server. Looks like they called [Microsoft's bluff](http://blogs.technet.com/jamesone/archive/2008/03/13/expensive-hypervisors-a-bad-idea-even-if-you-can-afford-them.aspx):

>If someone can show me a customer who is running, in production, a VMware VI3 Enterprise system with a 2:1 memory overcommit ratio on all the VMs, where spending the cost of VMware on RAM wouldn't remove the need to use overcommitment then I'll give... lets say $270 to their choice of charity.

Apparently, VMware feels they've met those criteria:

>So, James, the charity of choice is One Laptop Per Child. And just in case you believe that we've cherry picked a use case we'll be more than happy to connect you directly via phone to any one of the numerous customers we have leveraging memory overcommitment in their environment today.

Now things are really getting interesting. Stay tuned!

[1]: {{< relref "2008-03-14-virtualization-short-take-4.md" >}}
