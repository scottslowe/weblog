---
author: slowe
categories: Musing
comments: true
date: 2007-08-19T13:00:00Z
slug: the-linux-kernel-binary-modules-and-esx-server
tags:
- ESX
- Linux
- Virtualization
- VMware
title: The Linux Kernel, Binary Modules, and ESX Server
url: /2007/08/19/the-linux-kernel-binary-modules-and-esx-server/
wordpress_id: 506
---

As a systems engineer for a value-added reseller/systems integrator, I'm often called upon to perform both pre-sales work as well as post-sales work. I do a lot of work with [VMware](http://www.vmware.com/) (in case you hadn't guessed by now!), and so a lot of the pre-sales meetings in which I am involved often center around VMware, its product offerings, and the benefits that virtualizing with VMware can offer a customer. One question that frequently arises during these kinds of meetings is, "Isn't VMware based on Linux?"

What the potential customer is really asking is, "Is [VMware ESX Server](http://www.vmware.com/products/vi/esx/) based on Linux?", of course, and the proper party-line answer is that no, it's not based on Linux, that the vmKernel---the hypervisor at the heart of ESX Server---is custom, proprietary code written by Mendel Rosenblum and others when they founded VMware back in the mid-1990's. Yes, there is a Linux component to ESX Server called the Service Console, but it's used to provide an interface to the vmKernel. And when given that answer, customers nod in understanding and we move on. But is that the correct answer?

If you ask VMware, you'll be told that you gave the correct answer. But if you ask Christopher Hellwig, Linux SCSI storage maintainer and a top 10 contributor to the Linux kernel, he'll say that's not the correct answer. Instead, he'll insist that because vmKernel relies upon the Linux kernel in order to load, it is therefore a "derived work" and must fall under the same license as the kernel, GPLv2. So who's correct?

In truth, this is a very complicated matter. I'll refer you to this VentureCake article titled ["The VMware house of cards"](http://www.venturecake.com/the-vmware-house-of-cards/) for more details on the issue, as well as some links to additional information and viewpoints on the article. And perhaps just as helpful as the article are the comments to the article, which include [a response by Alan Cox](http://www.venturecake.com/the-vmware-house-of-cards/#comment-1282) (another Linux kernel maintainer) and [a response by Zach Amsden](http://www.venturecake.com/the-vmware-house-of-cards/#comment-1385) of VMware (although he does not speak for VMware officially). Go read that article and the comments, then come back here.

OK, done reading that article now?

As you can see from the VentureCake article, it's a fairly in-depth issue that has no clear answers. The true underlying issue, as I understand it, is this: Does vmKernel **_require_** the Linux kernel in order to boot? If so, if VMware can't show that vmKernel has "a life outside of Linux", then ESX Server is in violation of the GPL. If not, if VMware can show that vmKernel does not require the Linux kernel in order to operate or load, then all is well. The problem here, of course, is that only VMware knows the answer to this question, and any answer they provide will be suspect because people will believe they are covering up the truth.

Consider this excerpt from Zach Amsden's comment:

>First, the vmkernel is not a Linux kernel module. The vmkernel is a completely isolated and separate piece of software which is loaded by a module called vmnix. The vmkernel has no knowledge or understanding of Linux data structures or symbols, and as a necessary result, does not depend on the Linux kernel for any services whatsoever.

This would seem to pretty clearly dispel the idea that vmKernel relies upon the Linux kernel. However, consider this response from the article's author:

>If vmkernel is not ported from another OS and indeed depends on Linux to load (not run), by Linus Torvalds logic vmkernel is a derived work. Whether vmkernel continues to depend on Linux after it has loaded or not is irrelevant, if Linux copyright had to be infringed to start vmkernel in the first place.

So, because Zach didn't say that vmKernel doesn't rely upon the Linux kernel to _load_ (instead only that it doesn't rely on the Linux kernel to _run_), then the underlying question at the core of this issue still isn't answered. Some would say that people are being too picky, too selective in their interpretation of the words being used, but I suppose that in a delicate situation such as this that may be necessary.

The VentureCake article quotes Linus Torvalds a couple of items as it pertains to how we determine if a binary module for Linux should fall under the GPL:

>The reason I accept binary-only modules at all is that, in many cases, you have, for example, a device driver that is not written for Linux at all, but, for example, works on other operating systems, and the manufacturer wants to port that driver to Linux. But because that driver was obviously not derived from Linux (it had a life of its own regardless of any Linux development), I didnt feel that I had the moral right to require that it be put under the GPL, so the binary-only module interface allows those kinds of modules to exist and work with Linux.

And again:

>I personally consider anything a "derived work" that needs special hooks in the kernel to function with Linux (i.e., it is not acceptable to make a small piece of GPL-code as a hook for the larger piece), as that obviously implies that the bigger module needs "help" from the main kernel.

One question that comes up in my mind is this: what is meant by the term "load"? The VentureCake article keeps indicating that it's irrelevant what happens after vmKernel is initialized:

>Its possible to ditch, remove, or crash Linux after vmkernel has virtualized it - but you wouldnt be able to get to that stage without Linux being used to load vmkernel.

In my mind, the clearest application of Torvalds' logic of what makes a binary module a "derived work" (and therefore subject to the GPL) is not only whether it requires the Linux kernel to _load_, but also whether it requires the Linux kernel in order to _run_. Of course, I'm not a lawyer, not a kernel developer, and not a Linux expert (I'm not really even a VMware expert, despite what people say), so what I have to say about the matter doesn't really exist. And despite what people like Zach say, I also suspect that the open source community won't be happy until VMware open sources the vmKernel and they can look for themselves to see whether or not the Linux kernel was required by vmKernel. It seems to me that any other response, however well thought-out and logical, will only be interpreted as political posturing and concealing the truth.

One final question: Is this controversy helping to drive the development of the so-called "virtualization appliances" that will embed the hypervisor (vmKernel) into firmware, as [reported](http://searchservervirtualization.techtarget.com/originalContent/0,289142,sid94_gci1260992,00.html) by [various](http://www.thincomputing.net/newsitem3416.html) [sites](http://www.vmweekly.com/news/20070618/2/) [previously](http://www.virtualization.info/2007/06/vmware-esx-server-lite-edition-coming.html)?

**UPDATE:** This issue is starting to see broader attention, meriting a [comment by Gordon Haff of Illuminata](http://www.illuminata.com/perspectives/?p=347). His article is, in turn, being picked up by many other news aggregators.
