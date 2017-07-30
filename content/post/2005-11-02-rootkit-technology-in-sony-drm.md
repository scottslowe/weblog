---
author: slowe
categories: Rant
comments: false
date: 2005-11-02T19:16:28Z
slug: rootkit-technology-in-sony-drm
tags:
- Security
- Windows
title: Rootkit Technology in Sony DRM
url: /2005/11/02/rootkit-technology-in-sony-drm/
wordpress_id: 111
---

A [very in-depth article](http://www.sysinternals.com/blog/2005/10/sony-rootkits-and-digital-rights.html) by Mark Russinovich unveiled that DRM technology shipping with recent Sony BMG CDs actually installs [rootkit](http://en.wikipedia.org/wiki/Rootkit) components onto your system. Now, I don't know about you, but I don't like the idea of a vendor _unknowingly_ installing software on my computer(s) that contain rootkit components---especially when those rootkit components could be used by malicious software packages to hide themselves.

I'm glad to see that _eWeek_ [picked up the story](http://www.eweek.com/article2/0,1759,1880543,00.asp) as well. (Note that O'Reilly's ONLamp.net is also [mentioning it](http://www.oreillynet.com/pub/wlg/8269?CMP=OTC-6YE827253101).) Mark's research is fantastic (as always), but not many people will get exposed to this news through him alone. Computer geeks like myself would, of course, but the people that really need to know this are the non-geeks. Any reasonably computer-savvy user is likely to want to pick up your average CD (from any number of distribution channels, including [Amazon.com](http://www.amazon.com/)) and copy the music off the CD. There are a number of very valid reasons for this---ease of access, personal backup copy (ever scratch a CD?), etc. Copy-protected CDs limit your ability to do this without also installing stealthy rootkit components onto your system.

More links to other discussions on this topic can be found in section 8 of [this Wikipedia rootkit article](http://en.wikipedia.org/wiki/Rootkit) (also linked above).

In my opinion, this is just plain wrong. It's wrong to install this kind of software onto someone's system. I can understand their desire to want to protect their "intellectual property," but there's got to be a better way than installing rootkits on your customers' computers.
