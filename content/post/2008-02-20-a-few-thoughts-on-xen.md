---
author: slowe
categories: Rant
comments: false
date: 2008-02-20T21:38:42Z
slug: a-few-thoughts-on-xen
tags:
- Citrix
- ESX
- Virtualization
- VMware
- Xen
title: A Few Thoughts on Xen
url: /2008/02/20/a-few-thoughts-on-xen/
wordpress_id: 638
---

I'll start this blog posting with the disclaimer that I am not a Xen expert (at least, not yet). That being said, I had some questions, thoughts, rants, etc., that I wanted to get out of my system after a lengthy meeting with some [Citrix XenServer](http://www.citrix.com/English/ps2/products/product.asp?contentID=683148) folks today.

For any readers out there that are intimately familiar with the Xen hypervisor and, specifically, Citrix XenServer, feel free to provide factual, relevant technical information in the comments. Salespeople and marketing drones, spare us all the advertisting and public relations drivel.

First, the XenServer folks had a lot to say about how their 64-bit hypervisor was better than VMware's hypervisor because VMware's product was "only" a 32-bit hypervisor. My main response to that statement is this: what key benefit do you derive from being a 64-bit hypervisor? Access to more system RAM? No, VMware Infrastructure 3.5 supports 256GB of RAM, while XenServer 4.0.1 supports 128GB of RAM. Do you gain the ability to run 64-bit guests? Well, VMware's products can run 64-bit guests as long as the Intel VT/AMD V extensions are present. (Before you say that Xen doesn't need the extensions to run 64-bit guests, better stop and think about the requirements to run _unmodified_ guests of any bitness under Xen.) The ability to provide more RAM to guests? No, VMware beats Xen on that one, too--64GB versus 32GB. Hmmm...I'm stumped. So what does having a 64-bit hypervisor get you, then?

My second question centers around how the Xen hypervisor is so small and slim and petite versus VMware's bloated 2GB hybrid OS-hypervisor (their description, not mine). Of course, it's easy to make that distinction when you exclude the Linux-based Xen dom0 but include the Linux-based ESX Server Service Console. If want to compare hypervisor to hypervisor, fine; compare Xen itself with VMkernel. But don't compare the Xen hypervisor with the whole ESX Server product; you're not making an accurate comparison.

Third, let's talk about paravirtualization. The Citrix XenServer folks loved to talk about paravirtualization and how Xen uses it but VMware doesn't. What they seemed to omit, however, is the context in which we discuss paravirtualization. Does Xen use paravirtualization? Certainly---with guests that support paravirtualization, which does not include Windows. And they use paravirtualized drivers, to help optimize performance of the guest OS. Does VMware use paravirtualization? Yes, with guests that support paravirtualization, and that does not include Windows. Oh, and they do have paravirtualized drivers to help with guest OS performance. Where's the difference? Feel free to tell me I'm missing something here.

Forgive me if it seems like I'm bashing Xen; that is most definitely _not_ my intention. In fact, I'd love to get to know Xen much better than I do now. Products such as XenServer and Virtual Iron---both based on the open source Xen hypervisor---have the potential to make a huge splash in the market, and I for one want to be prepared to support these solutions when the time is right. I will tell you this, though: the _wrong_ way to get to guys like me is with misinformation and marketing. Talk honestly and openly about your product's strengths and weaknesses, and let your product stand, or fall, on its own merits.
