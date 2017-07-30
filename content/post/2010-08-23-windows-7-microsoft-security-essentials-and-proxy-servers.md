---
author: slowe
categories: Rant
comments: true
date: 2010-08-23T18:35:30Z
slug: windows-7-microsoft-security-essentials-and-proxy-servers
tags:
- Microsoft
- Networking
- Security
- Windows
title: Windows 7, Microsoft Security Essentials, and Proxy Servers
url: /2010/08/23/windows-7-microsoft-security-essentials-and-proxy-servers/
wordpress_id: 2043
---

On the recommendation of a number of Twitter users, I decided to install Microsoft Security Essentials (MSE) on a couple of laptops running 64-bit Windows 7. These laptops are used by my kids for their school work (they are home-schooled), and I just wanted to make sure that the laptops don't get infected with some nasty bug. More than a few Twitter users recommended MSE, so I figured it couldn't be all bad, right?

The install was quick and painless. And that's where the fun started. MSE wanted to do an update immediately; OK, that's fine. The problem is, it won't connect. I use a Squid proxy server to control outbound web access, so I figured that somewhere was a setting that told MSE to use a proxy server. There's nothing within MSE itself. Could it be that I had forgotten to configure Internet Explorer? I did make Firefox the default browser, after all. Nope, a quick check shows that the Internet Explorer settings are configured for the right outbound proxy as well. Both Internet Explorer and Firefox are working fine, so I know it's not the network, the proxy, or the firewall. It must be MSE itself.

Google turns up the [first part of the puzzle](http://all-things-pure.blogspot.com/2009/10/microsoft-security-essentials-network.html); even though your proxy support might be configured correctly for Internet Explorer (and thus most of the rest of Windows), MSE won't take those settings. Instead, you have to use `netsh`, like this:

	netsh winhttp import proxy source=ie

Unfortunately, in its efforts to be "helpful," Windows 7 won't allow you to run that command without elevated privileges. All you get when you try is a nondescript error message that vaguely implies that you don't have permission. However, instead of being able to elevate that one command (a la `sudo` in the UNIX/Linux/BSD world), you have to run the entire command prompt with administrative privileges, like explained [here](http://www.blogsdna.com/2168/windows-7-how-to-open-elevated-command-prompt-with-administrator-privileges.htm) (and probably countless other places on the 'Net).

Once you get a command prompt running with administrative credentials, then you can run the `netsh` command and it will successfully import the IE proxy configuration. Once the IE proxy configuration is successfully imported, then MSE will fetch updates from the Internet and function properly. _Wasn't that fun?_

This little episode brings up a couple questions/thoughts:

1. Why in the world _wouldn't_ MSE use IE's proxy configuration? Most of the rest of Windows does.

2. Even if Microsoft wanted MSE to have its own proxy settings, why force users down a rathole of command prompts and administrative privileges? Why not put it in the GUI?

3. Windows 7 has made great strides in making Windows more secure, but does this enhanced security posture come at the price of decreased flexibility for the power user?

4. If so, does Microsoft even care? After all, the default settings are probably fine for most users.

Anyway, there you have it. If you use a proxy server on your network and you also want to use MSE, you'll need to use `netsh` (with administrative privileges) to configure your proxy settings properly.
