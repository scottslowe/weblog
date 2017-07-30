---
author: slowe
categories: Musing
comments: false
date: 2006-06-09T17:40:35Z
slug: mac-firewall-wierdness
tags:
- Apple
- Macintosh
- Security
title: Mac Firewall Wierdness
url: /2006/06/09/mac-firewall-wierdness/
wordpress_id: 266
---

Over the last few days, I've been trying to fine-tune the settings for the firewall on my [PowerBook](http://www.apple.com/powerbook/). To configure the built-in ipfw firewall in [Mac OS X](http://www.apple.com/macosx/), I use a shareware utility called [Flying Buttress](http://personalpages.tds.net/~brian_hill/flyingbuttress.html) (formerly BrickHouse). Every time I think I have the rules configured the way they should work, something is messed up. There's some kind of wierdness going on.

For example, I had the following rules defined in the Expert mode interface of Flying Buttress (rule numbers have been randomized):

    add 11350 check-state
    add 11351 allow tcp from any to any in established
    add 11352 allow tcp from any to any out keep-state
    add 11353 allow udp from any to any out keep-state
    ...
    add 65000 deny log ip from any to any

I did _not_ have any rules to specifically allow DHCP/BootP or DNS, since it was my understanding that those rules would be covered by the stateful outbound rule 11353 above. What I found, though, was that DHCP and DNS information was being blocked and logged in Console.app.

I could kind of/sort of understand the DHCP thing, since my computer was apparently sending out a unicast UDP packet but the DHCP server was responding with a broadcast, and the broadcast didn't "match" the dynamic stateful rule created by ipfw. So, I added one rule:

    ...
    add 11450 allow udp from any 67 to any 68 in
    ...

That solved the DHCP/BootP issue. But what about DNS? My laptop was configured for a specific DNS IP address, and I could see the responses from the configured DNS server being blocked by rule 65000 above (the default "deny everything else" rule). What was up with that?

I understand the UDP is not a session-oriented protocol, and there isn't the same idea of "state" as with TCP sessions. However, all the research I performed (as well as my own experience with the pf firewall on OpenBSD) indicated that ipfw could maintain state for UDP conversations. So why was the return DNS traffic being blocked? I still don't know why even now, but eventually I had to do the same thing with DNS as I did with DHCP:

    ...
    add 11550 allow udp from any 53 to me 49152-65535 in
    ...

That finally allowed the return DNS traffic to be admitted. I also experimented at various points with changing rule 11351 (the "tcp established" rule) to _deny_ instead of allow, under the guise that the previous "check-state" rule would match a dynamic rule first, and that any traffic that didn't match the dynamic rule probably wasn't valid and should therefore be dropped anyway. However, that didn't work out so well, I think primarily because the lifetime for ipfw's dynamic rules was shorter than the communication windows of some of my applications, and so the dynamic rules were expiring before traffic could renew the rule. This particularly affected IM client traffic from my [Adium](http://www.adiumx.com/) client to MSN, AIM, Yahoo, and Google Talk.

If anyone has any insight on why exactly the rules weren't behaving as I expected, please let me know. Post in the comments for this post or even send me an [e-mail](mailto:scott.lowe@scottlowe.org) directly.
