---
author: slowe
categories: Rant
comments: true
date: 2007-02-28T12:07:34Z
slug: more-on-microsoft-versus-vmware
tags:
- ESX
- Microsoft
- Virtualization
- VMware
title: More on Microsoft versus VMware
url: /2007/02/28/more-on-microsoft-versus-vmware/
wordpress_id: 420
---

Microsoft's official response to the VMware complaints came in the form of an e-mail to Mary Jo Foley; you can read the [full response here](http://blogs.zdnet.com/microsoft/?p=283) (see the addendum at the bottom of the blog posting). In this response, Microsoft Virtualization Strategy General Manager Mike Neil claims:

>Microsoft believes the claims made in VMwares whitepaper contain several inaccuracies and misunderstandings of our current license and use policies, our support policy and our commitment to technology collaboration.

With regards to this response, I have to agree with Christian Mohn (of [h0bbel.p0ggel.org](http://h0bbel.p0ggel.org/)):

>While this seems to indicate that Microsoft will be looking to resolve the issues raised by VMware, I still see this email as nothing more than an attempt to further muddy the water as it really doesnt address anything at all.

I suppose that Microsoft could be correct that there are some misunderstanding and misconceptions here, although it seems unlikely given the way VMware quoted specific portions of text from the applicable Microsoft EULAs in their "white paper". (Isn't there a better term we can use? This wasn't a white paper.) Even so, if VMware is incorrect in their understanding of the restrictions in place in the EULAs, why didn't Microsoft respond sooner? As pointed out by a [blog posting](http://blogs.technet.com/windowsserver/archive/2007/02/27/VMyths.aspx) on the [Windows Server Division weblog](http://blogs.technet.com/windowsserver/default.aspx), these complaints are very much like the complaints issued by VMware back in November of last year during VMworld 2006 (see [this link](http://blogs.vmware.com/console/2006/11/licensing.html)). If VMware was incorrect then and is still incorrect now, why wait until now to try to address those inaccuracies?

As I responded to a commenter on [my previous article][1], the problem in my mind is not one of Microsoft being allowed to compete as fully and energetically as it can (an argument that the Windows Server Division weblog entry makes, quoting [this article from TechWorld](http://www.techworld.com/opsys/features/index.cfm?featureid=3195&amp;pagtype=samechan)), but rather are these competitive actions in violation of anti-trust laws? Are these the same kinds of actions that got Microsoft in trouble with the US Department of Justice and the European Union? (I'm not a lawyer, so I won't venture a guess.)

Furthermore, as again quoted by the Windows Server Divison themselves, a VMware deployment doesn't mean fewer Windows Server licenses (the quote is from [this Computerworld article](http://www.computerworld.com/action/article.do?command=viewArticleBasic&articleId=9011941&pageNumber=2)):

>Bruce McMillan, manager of emerging technologies at Solvay Pharmaceuticals Inc. in Marietta, Ga., is using Windows servers in a VMware virtualized environment. McMillan has read the white paper posted by VMware and said he thinks it "is there as a means to help educate people about what's going on."

>But he said he hasn't run into the issues outlined in the document. And just because Solvay is using virtualization technology, McMillan added, "it doesn't mean we're going to buy less [Windows] licenses."

If that is indeed the case---and my experience bears it out completely---what are the motivations behind Microsoft's licensing changes? And how does this affect specifications, such as VHD, that are covered by the [Microsoft Open Specification Promise](http://www.microsoft.com/interop/osp/default.mspx)? On one hand, the OSP states:

>Anyone is free to implement the specification(s), as they wish and do not need to make any mention of or reference to Microsoft. Anyone can use or implement these specification(s) with their technology, code, solution, etc.

Yet, according to [VMware's published complaints](http://www.vmware.com/solutions/whitepapers/msoft_licensing_wp.html), Microsoft is restricting the use of some VHDs to use with their virtualization products only (this is taken from the VMware site, which in turn quotes from a Microsoft EULA):

>You may install and use one copy of the software on your device of which you are running a validly licensed copy of Microsoft Virtual PC or Microsoft Virtual Server. You may not change or convert the virtual hard disk image from the VHD format.

Kind of seems like Microsoft is giving with one hand and taking with another, doesn't it? Developers are free to implement the VHD specification, but VHDs can only be used on Virtual PC or Virtual Server? What, then, is the point? (I will grant you that this complaint is a bit weak, since the text is taken from the EULA of a pre-configured VM available for download from Microsoft's web site. How much longer until this restriction isn't quite so specific, though?)

The other recurring theme I see in a lot of the articles out there about this topic is that VMware has nothing to complain about as they are currently the market leader, holding about 80% of the market share for server virtualization. Seems to me that Netscape held quite a commanding lead in the web browser share before Microsoft started bundling Internet Explorer with Windows. Is there a pattern here?

Besides, VMware isn't the only server virtualization vendor that's affected here. What about XenSource, SWSoft, or Virtual Iron? These licensing restrictions affect them, too. I think the only vendor that may be free of these restrictions _might_ be Novell, but only because of the recent patent agreement between Novell and Microsoft.

I guess it's the crazy stuff like this that drives people to use open source software---but that's a different story for a different day.

[1]: {{< relref "2007-02-27-return-of-the-old-microsoft.md" >}}
