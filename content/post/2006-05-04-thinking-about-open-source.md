---
author: slowe
categories: Musing
comments: false
date: 2006-05-04T19:20:37Z
slug: thinking-about-open-source
tags:
- Apache
- BSD
- Linux
- OSS
- Security
title: Thinking About Open Source
url: /2006/05/04/thinking-about-open-source/
wordpress_id: 241
---

Reading about the "Vulnerability Discovery and Remediation Open Source Hardening Project"---a security audit funded by the [Department of Homeland Security](http://www.dhs.gov/) to regularly review popular open source software (this article has [more information](http://www.eweek.com/article2/0,1895,1909946,00.asp))---got me thinking.

The [article that sparked my thinking](http://www.eweek.com/article2/0,1759,1956652,00.asp) discussed how a critical flaw had been discovered in the [X Window System](http://x.org/). This flaw was described as one of the most serious flaws uncovered to date. The flaw was corrected quickly, as is typical of most open source projects, but it wasn't really the flaw itself or the quick response to the flaw that really got to me. Instead, it was the fact that someone was _even able_ to search for such flaws.

The war-cry for open source proponents has always been, "Our software is more secure because more people have seen the code and reviewed it." Until now, I wasn't so sure about that argument; after all, how many people were like me? People who loved the projects, supported them in whatever way they could, but aren't developers? An ordinary guy like me can't contribute anything significant to an open source project because I don't know C, C++, C#, Java, Objective-C, or anything else. The fact that I _could_ review the code, until now, didn't really do me any good. Or so I thought.

Perhaps I'm just coming late to the party. Perhaps it's the involvement of the government, using my tax dollars, that has driven the idea home. Either way, now I see that the very right to review the source code is what makes open source projects so powerful in comparison with closed source software. As this DHS-sponsored project pores over millions of lines of code to find obscure bugs like the one described above, everyone (even Windows users) benefits. As security flaws, buffer overflows, etc., are corrected in software packages such as [Apache](http://www.apache.org/) (which runs the majority of web sites on the Internet, last I checked), [FreeBSD](http://www.freebsd.org/), the Linux kernel, [MySQL](http://www.mysql.org/), the Internet and our own private networks become more secure, more protected, and less likely to be used in attacks against others. _This_ is what the open source proponents have been so excited about, and why support for open source software is so strong.

This doesn't mean that open source projects are automatically "more secure," nor does it mean that we should all eschew all forms of commercial software in favor of open source equivalents. But it does mean that we do need to strongly consider open source equivalents, especially the high-profile ones, when developing solutions for customers. In my opinion, it would be a disservice otherwise.
