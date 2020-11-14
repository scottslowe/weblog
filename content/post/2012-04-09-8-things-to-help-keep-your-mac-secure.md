---
author: slowe
categories: Information
comments: true
date: 2012-04-09T09:00:00Z
slug: 8-things-to-help-keep-your-mac-secure
tags:
- macOS
- Security
title: 8 Things to Help Keep Your Mac Secure
url: /2012/04/09/8-things-to-help-keep-your-mac-secure/
wordpress_id: 2583
---

As the recent spate of Mac-specific malware shows, Mac OS X is not immune to security problems. (Not that this is really surprising to anyone.) To be honest, though, I was---until recently---fairly confident that my systems were reasonably secure. However, a Twitter conversation with security guru Christofer Hoff (aka [@Beaker](http://twitter.com/#!/beaker)) convinced me that I wasn't doing enough. The appearance of the Flashback.K trojan, which can install itself even without administrative privileges, confirmed that he was right---I wasn't doing enough. (No, I didn't get infected.)

Upon thinking about it a bit more, I realized that if _I_ wasn't doing enough as a pretty savvy user, then a lot of people probably weren't doing enough. So, here's a breakdown of my Mac defense strategy. Perhaps sharing what I'm doing with others will encourage them to improve their security posture as well.

1. **I use the BSD-level `ipfw` firewall.** Mac OS X is, at its core, built on FreeBSD. This powerful UNIX layer offers an equally powerful stateful firewall in the form of `ipfw`. If you aren't using `ipfw`, I'd encourage you to take a long, hard look at starting to use it. It provides a powerful ruleset to give you tremendous control over the types of traffic that are allowed into (and out of) your Mac. To help encourage people to use it, I recently published an article on [how to configure ipfw on Mac OS X][1].(Keep in mind that Mac OS X 10.7 "Lion" prefers `pf` instead of `ipfw`. I hope to post an article on that soon as well.)

2. **I use the built-in Mac OS X application-level firewall.** Mac OS X ships with a pretty GUI for a built-in application-level firewall in System Preferences. I recommend that you turn it on, and select which applications you want to accept incoming connections. Some people have asked "Why both firewalls?" This is a fair question. The built-in application-level firewall simply allows or denies inbound traffic on a per-application level, but doesn't---to my knowledge---offer any more granularity than that. Using the built-in application-level firewall in conjunction with the BSD-level `ipfw` (or `pf`) firewall gives you the ability to specify which source addresses or networks are allowed to make connections to applications. This means that you can allow iTunes connections at the built-in firewall layer, and then use `ipfw` (or `pf`) to only allow connections from your home network subnet.

3. **I use an outbound application-level firewall.** The built-in Mac OS X firewall in System Preferences only controls inbound traffic. What about outbound traffic? Do you know what processes and applications on your system are communicating with the outside world? I use [Little Snitch](http://www.obdev.at/products/littlesnitch/index.html), which I believe to be an excellent choice in this area. (No, I don't have any affiliation with Objective Development.) Little Snitch gives you the visibility to know what applications and processes are communicating and on which protocols and ports.

4. **I use an account without administrative privileges for my day-to-day use.** While this won't thwart all security problems---Flashback.K still works, for example---it's still a good idea. I also recommend that you only install applications using a separate account with administrative privileges. This forces you to log off, log on as the administrative user, then install your application(s). While this is a bit of a hassle, the security trade-off is, in my opinion, worth it.

5. **I disabled the opening of "Safe" files.** Safari has this feature enabled by default. I recommend that you turn it off, and check to make sure it's turned off in other applications as well.

6. **I use an AV application.** Yes, yes, I know---Macs don't get viruses. Tell that to the 600,000 Macs infected with the Flashback trojan. And while Flashback isn't _technically_ a virus, at this point you're just splitting hairs. I'm using the free [Sophos AV Home Edition for the Mac](http://www.sophos.com/en-us/products/free-tools/sophos-antivirus-for-mac-home-edition.aspx) and feel that it is pretty good, but there are numerous others. Find one and use it. (This is a recent addition to my own security strategy.)

7. **I do my best to stay updated.** I encourage you to run Software Update on a regular basis. If you've followed the advice of #4, this means you'll need to log in as an administrator and run Software Update. Make it a point to check regularly.

8. **I don't run the standalone Adobe Flash Player.** Instead, I use Google Chrome when Flash is required, which comes with its owned patched version of Adobe Flash that is generally regarded (last time I checked) to be a bit safer than the standalone version of Flash. Yes, this means that I need to switch back and forth between browsers (Safari for day-to-day use, Chrome for Flash use), but this is a task that AppleScript easily solves.

While these 8 things aren't going to guarantee that my Mac (or yours, should you choose to follow them as well) will never be exploited, I do feel that they provide a reasonable level of protection. Safe computing (and safe browsing) is still required; no amount of security can protect against stupidity. But when combined with security awareness and safe computing/browsing, I feel that these measures will provide the level of protection that I need.

(BTW, there are other network-level protections that I have in place as well, but I didn't include them here as the focus of this article is on the Mac itself.)

If you have any additional suggestions for helping keep your Mac secure, please feel free to speak up in the comments. Every suggestion can help!

[1]: {{< relref "2012-04-05-setting-up-ipfw-on-mac-os-x.md" >}}
