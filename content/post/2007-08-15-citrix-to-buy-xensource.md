---
author: slowe
categories: News
comments: true
date: 2007-08-15T10:30:40Z
slug: citrix-to-buy-xensource
tags:
- Citrix
- Microsoft
- Virtualization
- VMware
title: Citrix to buy XenSource
url: /2007/08/15/citrix-to-buy-xensource/
wordpress_id: 504
---

It's odd how things work out sometimes. I was in a meeting earlier today with our VMware channel SE and some other colleagues on our VMware team, and the subject of VMware's relationship with Citrix came up. The VMware SE went on to explain that there was no head-to-head competition between [VMware](http://www.vmware.com/) and [Citrix](http://www.citrix.com/), and that the two companies' products were, in many cases, complementary. Fine, no big deal; I can buy that. I've seen a lot of instances where virtualizing Citrix Presentation Server offered value to the customer, even if it did mean a drop in the total number of users that could be supported on each (virtual) instance of Presentation Server. (The benefits of snapshots, cloning, VMotion, etc., are pretty compelling in some environments, and outweigh the decreased user session counts. But alas, I digress...)

Somehow (I think it was when the topic of no formal technology partnerships being formed between VMware and Citrix came up), I mentioned that I had been seeing the rumors of a Citrix acquisition of [XenSource](http://www.xensource.com/), the commercial backer of the open source Xen hypervisor. Up until now, it had been just [rumors](http://www.brianmadden.com/content/article/Should-Citrix-buy-a-hypervisor), but the [buzz](http://www.theregister.co.uk/2007/08/13/citrix_buy_xensource/) grew stronger over the last few days until Alessandro Perilli of virtualization.info broke the news that (supposedly) [Citrix will announce the acquisition of XenSource](http://www.virtualization.info/2007/08/citrix-to-announce-xensource.html) tomorrow. Quoting from the [original article at _The Register_](http://www.theregister.co.uk/2007/08/14/xensource_goes_citrix/):

>Citrix will announce its acquisition of XenSource tomorrow, The Register has learned. In a bid to expand its software management play, Citrix will grab the developer of the open source Xen hypervisor. The deal will give XenSource heftier corporate backing needed to compete against VMware. Meanwhile, Citrix will be able to expand its own virtual desktop effort revealed in April and its flagship software streaming service.

Personally, I second _The Register's_ question: how could Microsoft let this happen? With delays in Windows Server Virtualization (expected 180 days after the debut of [Windows Server 2008](http://www.microsoft.com/windowsserver2008/default.mspx) in February 2008) and that product still lacking key features such as live migration, Microsoft's best entry into controlling the virtualization stack would have been to purchase XenSource and gain control of Xen. However, I can't help but think that Xen's open source roots would have made an acquisition by Microsoft a bitter one, indeed.

In any case, the virtualization landscape is set to change pretty rapidly now. With VMware's IPO complete, XenSource's release of XenEnterprise 4.0, the acquisition of XenSource by Citrix, and the looming arrival of Windows Server Virtualization next year, the virtualization industry is in for some interesting times. (For some organizations---like [Virtual Iron](http://www.virtualiron.com/), [Novell](http://www.novell.com/), and [Red Hat](http://www.redhat.com/)--it will be more interesting than for others.)

Read all of Alessandro's thoughts regarding the XenSource acquisition in his [original article](http://www.virtualization.info/2007/08/citrix-to-announce-xensource.html).

&lt;aside&gt;By the way, does anyone else besides me wonder how in the world Alessandro gets hold of all this information? I wish I had his connections!&lt;/aside&gt;

**UPDATE:** News of the formal announcement is all over the Internet---see [here](http://www.informationweek.com/news/showArticle.jhtml?articleID=201800258), [here](http://www.citrix.com/lang/English/lp/lp_680809.asp), or [here](http://news.com.com/8301-10784_3-9760160-7.html). Once again, my hat is off to Alessandro for outstanding work!
