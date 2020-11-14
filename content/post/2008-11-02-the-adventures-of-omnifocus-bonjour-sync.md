---
author: slowe
categories: Explanation
comments: true
date: 2008-11-02T17:51:00Z
slug: the-adventures-of-omnifocus-bonjour-sync
tags:
- Apple
- iPhone
- macOS
- Networking
- Wireless
title: The Adventures of OmniFocus Bonjour Sync
url: /2008/11/02/the-adventures-of-omnifocus-bonjour-sync/
wordpress_id: 1007
---

I've made no secret of the fact that I use and enjoy [OmniFocus](http://www.omnigroup.com/applications/omnifocus/), a GTD application from the fine folks at OmniGroup. (Just for the record, I also use and enjoy [OmniGraffle](http://www.omnigroup.com/applications/omnigraffle/) and [OmniOutliner](http://www.omnigroup.com/applications/omnioutliner/). Doesn't that qualify me for a free license to [OmniPlan](http://www.omnigroup.com/applications/omniplan/)?) In fact, one of the driving factors for purchasing my iPhone---aside from the fact that it's cool and I wanted one---was the fact that OmniGroup also had an [iPhone version of OmniFocus](http://www.omnigroup.com/applications/omnifocus/iphone/).

In addition, version 1.5 of OmniFocus for Mac (OF/Mac)--currently at RC2 status---will synchronize with OmniFocus for iPhone (OF/iPhone) via MobileMe, WebDAV, or Bonjour on local Wi-Fi. This weekend, version 1.1 of OF/iPhone finally got approved and pushed out to the App Store servers. The new version adds Bonjour sync over local Wi-Fi, meaning that it's now possible to quickly and easily synchronize the OmniFocus database on my laptop with the OmniFocus database on my iPhone. Sweet! (Version 1.1 of OF/iPhone also improves performance and has a few other improvements as well.)

Unfortunately, in the late hours last night, I just couldn't get things to work. No matter how hard I tried, OF/iPhone just wouldn't synchronize with OF/Mac. It would see my laptop advertising via Bonjour, but wouldn't synchronize. My first suspicion proved to be a good one: the IPFW firewall on my laptop. (I use a custom IPFW ruleset in addition to Little Snitch to control traffic moving into and out of my laptop.) I was able to confirm that the firewall was blocking the traffic with this command:

	sudo ipfw show

Sure enough, I saw the default deny rule's counters incrementing every time I tried to synchronize. With this command I quickly disabled the IPFW rules:

	sudo ipfw flush

OK, that got me a bit farther, but OF/iPhone was still reporting an error. The error was different this time, though, so I was convinced that I had made some progress. Unsure of what could be wrong next, I tested a hunch and logged into the Squid proxy server that controls outbound HTTP/HTTPS traffic and checked the access logs. Bingo---OF/iPhone was hitting the proxy every time I tried to synchronize. But how to fix that? My network is configured such that the Cisco PIX firewall won't allow any traffic out that doesn't first go through the Squid proxy, so turning the proxy settings off on my iPhone would be a temporary fix at best. Nevertheless, to test my settings I disabled the proxy settings on the iPhone (under Settings > Wi-Fi and scroll all the way to the bottom). That did it---OF/iPhone was now able to synchronize with my laptop. Rather quickly, too, might I add, which is a definite improvement over the WebDAV setup I'd been using previously.

But I was still left with two problems: a) the firewall on my laptop was disabled; and b) the proxy settings on my iPhone were disabled. So I set out to fix those two issues.

Fixing the firewall should be easy, right? Just find out what port OF/Mac listens on and create a firewall rule to allow the traffic. It would be easy, _except_ for the fact that the port on which OF/Mac listens changes every time the application launches. OmniGroup, if you're listening: change this to a static port, PLEASE! Or at least provide some sort of hidden preference that would allow geeks like me to make it use a static port. As it stands right now, I had to use a rule that opens up a broad range of ports. That really, _really_ stinks. OK, firewall issue resolved.

The proxy issue proves to be more challenging. There's no way to configure the proxy to ignore the traffic; that has to be done client side. Unlike the full-blown version of Mac OS X, there's no option in the iPhone to ignore certain network ranges or certain DNS domains. But---and here's the kicker---the iPhone does support automatic proxy configuration via a PAC file. So I create a very simple PAC file like this:

	function FindProxyForURL(url, host)  
	{  
	if (isInNet(host, "172.16.1.0", "255.255.255.0"))  
	{  
	return "DIRECT";  
	}  
	else  
	{  
	return "PROXY server.domain.com:3128";  
	}  
	}

The first round of testing didn't go so well; the Apache HTTPd configuration on my server didn't allow files with a .PAC extension. Oops! After fixing that problem, testing from my laptop went very well. A few seconds later, I had my iPhone reconfigured with the PAC file. And it worked! I was able to successfully synchronize OF/iPhone with OF/Mac while still maintaing access to Internet-based resources. And since the PIX firewall won't allow traffic direct from the iPhone to the Internet, I know that the proxy is still involved in those connections. Reviewing the Squid proxy's access logs also confirmed that the iPhone was not hitting the proxy during synchronization attempts.

Problem all solved, right? Well...not exactly. I left the PAC configuration on my laptop until this morning, when I fired up [Adium](http://www.adiumx.com/). Bam! Adium throws an error message stating that it doesn't support PAC files. I reconfigured my laptop back to my old configuration and testing the OF/iPhone-to-OF/Mac synchronization again, and it still worked. It appears that as long as the iPhone's proxy configuration is correct, then the synchronization will work.

So, lessons learned:

* OF/Mac uses a dynamically assigned port on which it listens for synchronization requests. This means that users with an IPFW firewall configuration will have to open up a wide range of ports until OmniGroup gives us the option to statically assign a port. (Hint, hint...)

* HTTP proxy settings on the iPhone will interfere with OF/iPhone synchronization, but that can be solved with a PAC file as described above.

Other than this adventure, I am thus far quite pleased with the update to OF/iPhone. If you are unhappy with the earlier version---the most common complaint was speed, or lack thereof---you owe it to yourself to check out version 1.1. It's much improved, in my opinion.
