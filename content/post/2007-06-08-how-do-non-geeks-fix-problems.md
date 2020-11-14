---
author: slowe
categories: Rant
comments: true
date: 2007-06-08T16:50:10Z
slug: how-do-non-geeks-fix-problems
tags:
- Cisco
- macOS
- Networking
- Security
title: How Do Non-Geeks Fix Problems?
url: /2007/06/08/how-do-non-geeks-fix-problems/
wordpress_id: 470
---

For the last few weeks, I've been implementing a [Squid](http://www.squid-cache.org/) proxy server (with [SquidGuard](http://www.squidguard.org/) content filtering) to help control outbound web access from my home network. Basically, I wanted to make sure that the kids weren't accessing content that was inappropriate. So, after making sure that the proxy server was working as expected, last night I locked down the [Cisco PIX firewall](http://www.cisco.com/en/US/products/hw/vpndevc/ps2030/) (OK, I'm a geek---what can I say?) to only allow outbound HTTP/HTTPS traffic from the proxy server itself. (I suspect that one of my daughters had discovered that she could bypass the proxy, hence the need to lock down the firewall. She's got a surprise coming.)

Upon arriving home from work today, I booted up my laptop and launched my suite of applications and sites ([Camino](http://www.caminobrowser.org/), [NetNewsWire](http://www.newsgator.com/individuals/netnewswire/), [Adium](http://www.adiumx.com/), [Cocalicious](http://alittledrop.com/cocoalicious/), webmail for the office, a few other key web sites, etc.). I was greeted with a prompt to enter the password for two [MSN](http://www.msn.com/) accounts that I use for instant messaging with Adium. I entered the password. It prompted me again. Puzzled, I typed the password again, more slowly to make sure that I had every character right. Still wouldn't connect. What was going on?

Now suspicious that someone had defrauded my accounts, I logged in to the accounts via an HTTPS connection in a Web browser. No, the password was right; the accounts were OK. So why wouldn't Adium connect?

Next, I reviewed the account settings in Adium while also performing a [Google](http://www.google.com/) search. The settings looked right (most notably, the "Connect via HTTP" option was _unchecked_), and the Google search turned up an [Adium Trac ticket](http://trac.adiumx.com/ticket/6910) about MSN connectivity with a proxy server. At that point, the light bulb came on---I must need to configure Adium to use the systemwide proxy settings in [Mac OS X](http://www.apple.com/macosx/). A quick couple of clicks later I was done, but Adium still wouldn't connect. Huh?

&lt;aside&gt;I still haven't determined if configuring Adium to use the systemwide proxy settings failed due to a content violation, an error with the proxy configuration, or a bug in Adium. Nothing was logged in the blocked sites log on the proxy, though, so I'm leaning toward one of the latter two.&lt;/aside&gt;

OK, then, let's go old-school on this thing. I logged into my PIX firewall, issued a `show access-list` command to get the current traffic matching statistics, and tried to connect again. Another `show access-list` command, and it became painfully clear that the outbound rule blocking HTTPS access from everyone except the proxy was causing the problem.

&lt;rant&gt;If an application says that it uses a certain set of protocols or ports, **_DON'T_** use an entirely different set of protocols and ports. It drives people like me crazy! (Not to mention it flies in the face of trying to establish reasonable security policies.)&lt;/rant&gt;

Back in the PIX firewall again, I entered a `debug packet` command to echo HTTPS packets exiting the network, and tried connecting to my MSN account from Adium again. Yep, there it is---a couple of different IP addresses (65.54.179.228 and 65.54.183.203) showed up in the debug output every time I tried to connect. After modifying the access-list to allow HTTPS traffic to those specific addresses, Adium connected without any further problems.

Now what would an "ordinary" person have done in this situation? Would an "ordinary" (and by that I mean non-network engineer or non-geek individuals) person have known to check their firewall? Or to issue a command to have the firewall tell you about blocked packets so that they could determine exactly what was being blocked and why?

Granted, ordinary people probably won't have PIX firewalls at their house, and probably wouldn't be locking down outbound traffic via access lists. Still, doesn't this tell us something? Shouldn't applications adhere to the protocols and ports they say they are using? Shouldn't applications be intelligent enough to provide an error message that describes the underlying problem? After all this progress in computer hardware, applications, and networking, and we are still unable to accurately diagnose problems without going to multiple sources? Or am I just expecting too much?
