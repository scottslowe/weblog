---
author: slowe
categories: News
comments: true
date: 2007-08-21T13:58:36Z
slug: xensource-bidding-war
tags:
- Citrix
- Microsoft
- Virtualization
title: XenSource Bidding War
url: /2007/08/21/xensource-bidding-war/
wordpress_id: 507
---

[Several](http://www.p2vd.com/?p=184) [virtualization-related](http://geekswithblogs.net/WallabyFan/archive/2007/08/21/114830.aspx) blogs are commenting on this article by The Deal titled ["Redmond to spoil XenSource buy?"](http://www.thedeal.com/servlet/Satellite?pagename=NYT&c=TDDArticle&cid=1186574743494). Is [Microsoft](http://www.microsoft.com/) preparing to launch a bidding war for [XenSource](http://www.xensource.com/), or are they content to allow [Citrix](http://www.citrix.com/) to gobble up the small virtualization company?

>'It is quite likely that Microsoft may put in a competing bid on XenSource to the level of $1 billion,' said Trip Chowdhry, senior software analyst at Global Equities Research. 'And I would say if Microsoft puts in a bid, IBM won't stand still.'

But not everyone is convinced:

>'I see nearly zero possibility Microsoft steps in and bids,' said Walter Pritchard, a financial analyst at Cowen and Co. LLC. 'Microsoft is already too far down the road with its own technology in this area.

I have to say that I'm with the naysayers on this one. Microsoft has already invested so much in Windows Server Virtualization (aka "Viridian"), their own hypervisor, that attempting to purchase XenSource would be tantamount to admitting their own hypervisor is somehow lacking. And what would they get, anyway? The Xen hypervisor itself is open sourced, so the purchase would only get them access to XenSource's management products that run on top of the Xen hypervisor, providing functionality like XenMotion (live migration), hot-plug of resources, etc. Would they fork the Xen hypervisor and make it their own? That might prove to be a viable short-term strategy, but I suspect that it would come back to bite them in the long-term.

Besides, Citrix is already stating that their purchase of XenSource is not intended to help them compete directly against Microsoft. Observe this quote from the [official Citrix press release](http://www.citrix.com/English/NE/news/news.asp?newsID=680808&ntref=hp_article_headlines_US) regarding the XenSource acquisition:

>...XenSource has built a strategic relationship with Microsoft designed to ensure broad interoperability between XenSource products and the upcoming Microsoft Windows hypervisor, code named "Viridian". This relationship complements and broadens the successful partnership between Citrix and Microsoft...

And this one:

>Citrix currently intends to distribute the XenEnterprise product line through more than 5,000 channel partners with proven expertise in enterprise datacenter solutions built on the Windows Server platform.

Also, refer to this quote by Simon Crosby, CTO of XenSource, during [an interview with virtualization.info](http://www.virtualization.info/2007/08/xensource-acquisition-q-with-simon.html):

>Citrix has a long history of embracing the Microsoft Windows platform and adding value on top of it and will do the same with the upcoming Viridian platform.

I could go on, but that seems sufficient. Citrix has far too much to lose, in my opinion, by alienating Microsoft; after building its business on the back of Microsoft's product offerings for years, that would seem an awful lot like biting the hand that feeds you. No, I think Citrix will take the Xen hypervisor and embed it into a future version of Presentation Server or Citrix Desktop Server, and then shape the XenEnterprise management software to work not only with the Xen hypervisor but also with Windows Server Virtualization, whenever Microsoft finally gets that into the market. This reasonably safe strategy allows them to gain immediate value by enhancing their own product offerings and positions them to be a value-added provider once Microsoft's virtualization solution finally reaches the masses.
