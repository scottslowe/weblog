---
author: slowe
categories: Information
comments: true
date: 2009-01-08T16:38:13Z
slug: my-ubuntu-mac-article-is-popular-i-guess
tags:
- Linux
- macOS
- Music
- Ubuntu
- VPN
title: My Ubuntu-Mac Article is Popular, I Guess
url: /2009/01/08/my-ubuntu-mac-article-is-popular-i-guess/
wordpress_id: 1129
---

Apparently my [Ubuntu-Mac integration article][1] is quite popular; it's been picked up for re-publication on a couple of different sites:

[http://linux.sys-con.com/node/803618](http://linux.sys-con.com/node/803618)  

[http://opensource.sys-con.com/node/803618](http://opensource.sys-con.com/node/803618)

Cool! I hope the article is useful to others.

As a quick follow-up to that article, you may recall that I ran into a [strange issue with OpenVPN and mt-daapd][2] prior to the home network rebuild. I just finished installing OpenVPN last night and, anticipating the problem, did some digging to see how I'd fix the problem this time around. Turns out there's nothing to worry about; Avahi skips point-to-point interfaces by default, and OpenVPN tags its interfaces as point-to-point. So, everything works as expected.

[1]: {{< relref "2009-01-02-ubuntu-and-mac-os-x-integration.md" >}}
[2]: {{< relref "2008-12-26-openvpn-and-mt-daapd.md" >}}
