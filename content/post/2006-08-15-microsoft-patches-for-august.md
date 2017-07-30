---
author: slowe
categories: News
comments: true
date: 2006-08-15T18:13:51Z
slug: microsoft-patches-for-august
tags:
- Microsoft
- Office
- Security
- Windows
title: Microsoft Patches for August
url: /2006/08/15/microsoft-patches-for-august/
wordpress_id: 321
---

This MSRC blog posting outlines the [patches that were released](http://blogs.technet.com/msrc/archive/2006/08/09/445600.aspx) last Tuesday, and provides links to the security bulletins for each patch.

&lt;aside&gt;An interesting statistic: according to [this article](http://www.darkreading.com/document.asp?doc_id=101047&f_src=darkreading_section_318), [Microsoft](http://www.microsoft.com/) has released more patches in the first 8 months of 2006 than in all of 2004 and 2005 _combined._ I don't know if that makes me feel more secure---in that they are patching more vulnerabilities instead of not patching them---or less secure.&lt;/aside&gt;

The [MS06-040](http://www.microsoft.com/technet/security/Bulletin/MS06-040.mspx) bulletin is the one critical patch that is really getting everyone's attention; this is the one that is deemed to be "wormable," capable of creating a self-replicating worm such as Blaster or Slammer. In fact, there were [reports of limited attacks](http://www.eweek.com/article2/0,1759,2002966,00.asp) using the exploit patched by MS06-040. According to a [follow-up posting](http://blogs.technet.com/msrc/archive/2006/08/13/446268.aspx) on the MSRC blog, these attacks were limited in nature and only affected Windows 2000 (see this [MSRC posting](http://blogs.technet.com/msrc/archive/2006/08/13/446396.aspx) as well).

Fortunately, the MS06-040-based attacks are fairly straightforward to defeat, especially for traffic coming from the Internet. By blocking TCP ports 139 and 445 at the perimeter, these attacks are defeated. Of course, that does nothing for the kind of internal infections that were so common with Blaster (which was often carried in by a laptop and then spread behind the firewall). This article has more information on [protecting against the MS06-040 attack](http://www.darkreading.com/document.asp?doc_id=101315&f_src=darkreading_section_318).

One other patch ([examined in more detail here](http://www.eweek.com/article2/0,1759,2003395,00.asp)) fixes the zero-day PowerPoint exploit that garnered attention back around the middle of July.

Because exploit code already exists for both of these vulnerabilities, many security experts are recommending that organizations give priority to getting these patches rolled out to affected systems.
