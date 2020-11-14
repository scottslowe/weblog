---
author: slowe
categories: Musing
comments: true
date: 2007-06-27T17:30:00Z
slug: statistically-secure
tags:
- Apple
- Linux
- macOS
- Microsoft
- Security
- Solaris
- Sun
- Windows
title: Statistically Secure
url: /2007/06/27/statistically-secure/
wordpress_id: 478
---

I'll start out by saying that I am neither a security expert nor a statistician. With that disclaimer in hand, I wanted to briefly share my thoughts on the "days of risk" assessment that has recently been used to compare the security of Windows, Linux (Red Hat and SuSE), Mac OS X, and Sun Solaris. Before continuing, I encourage you to have a look at the actual report itself, along with a few related articles:

* [Days-of-risk in 2006 : Linux, Mac OS X, Solaris and Windows](http://blogs.csoonline.com/days_of_risk_in_2006)

* [Days of Risk in 2006 : Client OS Products](http://blogs.csoonline.com/node/365)

* [Recount: Windows Still Safest, Tops Mac OS X, Linux and Sun Solaris - But are statistics a true measure of security?](http://news.softpedia.com/news/Recount-Windows-Still-Safest-Tops-Mac-OS-X-Linux-and-Sun-Solaris-57433.shtml)

* [Windows v Linux - Days of risk in 2006](http://blogs.zdnet.com/security/?p=306)

In summary, the Days-of-Risk (DoR) assessment showed that Microsoft patched vulnerabilities in Windows more quickly than Red Hat, Novell, Apple, or Sun patched vulnerabilities in their products. This is true even when only High Severity issues are taken into consideration, although the gap between Microsoft and the other vendors narrowed in that analysis (with the exception of Sun).

OK, that's all well and good, but we all know statistics can be made to show just about anything. I'm not saying that Mr. Jones deliberately limited his data to present a favorable outcome for Microsoft; Microsoft has done a very admirable job of improving their security responsiveness, and in that regard the other vendors would do well to improve their own responsiveness to the disclosure of security vulnerabilities. No, my thoughts are more centered on the question: Is this data the _right data_ to accurately and objectively represent the security profile of an operating system?

I would contend that, in addition to DoR, information on the following areas would also need to be included in order to more accurately depict an operating system's security profile:

* Number and severity of exploits published or otherwise made available for vulnerabilities

* Number of viruses, trojans, rootkits, or other malware readily available or in active circulation

Now, before you say something like "Well, of course Windows is going to have more viruses and more exploits because it has a larger installed base!", let me also say that these values should be correlated and weighted according to the installed base of the operating system as well. This allows the values to account for the fact that Windows is in use by a much larger base of users than Linux, Solaris, or Mac OS X.

Again, I'm not a statistician, but surely there's a way to correlate this data (including DoR) and start presenting some sort of objective guide, based on measurable facts, regarding the security of an operating system. Then the vendors (Microsoft, Apple, Novell, Sun, Red Hat, and others) can stand on equal ground and be able to make some sort of reasonable comparison regarding the security of each product. Isn't that what we really need anyway?
