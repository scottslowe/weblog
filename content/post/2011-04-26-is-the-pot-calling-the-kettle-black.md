---
author: slowe
categories: Rant
comments: true
date: 2011-04-26T09:17:40Z
slug: is-the-pot-calling-the-kettle-black
tags:
- HyperV
- Microsoft
- Virtualization
- VMware
- vSphere
title: Is the Pot Calling the Kettle Black?
url: /2011/04/26/is-the-pot-calling-the-kettle-black/
wordpress_id: 2279
---

A recent post by Microsoft on the Windows Virtualization Team Blog titled ["Hyper-V VM Density, VP:LP Ratio, Cores and Threads"](http://blogs.technet.com/b/virtualization/archive/2011/04/25/hyper-v-vm-density-vp-lp-ratio-cores-and-threads.aspx) caught my eye this morning as I was scanning my RSS feeds. In this post, the author (the anonymous WSV_GUY) works through the idea of cores vs. logical processors. The distinction here, in case you didn't already know, is that many modern multi-core CPUs also support symmetric multi-threading (SMT, also referred to as hyperthreading), which means that an eight core CPU can actually process 16 threads simultaneously and would therefore be considered to have 16 logical processors.

&lt;aside&gt;I can see where this might be an area of some confusion; in fact, I was just discussing hyperthreading with a colleague last week. In my opinion, it's far more accurate to refer to current-generation functionality as SMT than hyperthreading, but that's another story for another day.&lt;/aside&gt;

What really caught my eye was the part of the article where the author compares and contrasts Microsoft's approach and others' approaches. I've taken a screenshot here in case the original article changes. Keep in mind that the article is based on the discussion of _maximum_ virtual CPUs (or VPs, as WSV_GUY calls them) per logical CPU:

![Microsoft blog quote](/public/img/wsv-article-quote.png)  

**Figure 1. Screenshot of Microsoft blog post**

So, two things pop to mind immediately. Let's take these in order.

First---since it's fairly obvious that Microsoft is targeting VMware as the primary "other virtualization vendor"--it should be noted that VMware does not consistently use cores as their unit of measure. As a point of proof, I present to you this screenshot taken from VMware's Configuration Maximums document for vSphere 4.1 (available in PDF [here](http://www.vmware.com/pdf/vsphere4/r41/vsp_41_config_max.pdf)). I've taken the liberty of highlighting the two key takeaways:

![VMware configuration maximums document](/public/img/vmw-config-max-shot.png)  

**Figure 2. Screenshot of VMware configuration maximums document**

As you can see from the documentation, VMware inconsistently switches back and forth from logical CPUs to cores. From that perspective, VMware has some work to do on presenting consistent messaging and consistent documentation. Point taken. VMware, are you listening?

But that's not really my major beef with the article.

The second thing I noted was the statement in the Microsoft blog (see Figure 1) about "Vendor A" and statements about ratios. Remember that the entire blog post appears to be about maximum ratios: "Vendor A response 16:1 (with the qualifier that your mileage will vary)". It seems to me that the author is referring to the statement at the bottom of the VMware configuration maximums document (see Figure 2) that discusses the _achievable_ number of virtual processors per core. However, we're not talking about _achievable_ ratios, we're talking about _maximum_ ratios, right? Or are we?

Although the Microsoft author appears to ding VMware for making a statement about achievable ratios in an article discussing maximum supported ratios, later in the same article the author does the same thing (the emphasis is mine):

>You can see that even with an 8:1 VP to LP ratio (or 16:1 VP: Core, if you prefer), Hyper-V supports very dense VM configurations. Even on a server with two physical processors, Hyper-V supports a staggering number of virtual machines (up to 256). The limiting factor won't be Hyper-V. **_It will be how much memory you've populated the server with and how well the storage subsystem performs._**

Sounds to me like Microsoft is saying that they have a maximum ratio of virtual CPUs to logical CPUs, but that the actual ratio can you achieve (the _achievable_ ratio?) might be less than that. How is that any different from the statement in VMware's configuration maximums document? How is Microsoft's "approach" with regard to ratios any different, better, or clearer for the customer? Yes, VMware's documentation is inconsistent. But when it comes to maximum ratios vs. achievable ratios, it seems to me that the pot is calling the kettle black.

If I'm off or I'm overlooking something, please let me know by speaking up in the comments. Please use full disclosure of your employer where that employment might affect your viewpoint. Thanks!
