---
author: slowe
categories: Information
comments: false
date: 2005-08-05T10:55:13Z
slug: ipsec-mac-os-x-and-windows-server-2003
tags:
- IPSec
- Windows
- macOS
- Encryption
- Interoperability
- Networking
title: IPSec, Mac OS X, and Windows Server 2003
url: /2005/08/05/ipsec-mac-os-x-and-windows-server-2003/
wordpress_id: 66
---

For quite some time now, a minor task I've been experimenting with is establishing transport mode IPSec security associations between [Mac OS X](http://www.apple.com/macosx/) and [Windows Server 2003](http://www.microsoft.com/windowsserver2003/default.mspx). I've been using a freeware IPSec client called [IPSecuritas](http://www.lobotomo.com/products/IPSecuritas/). Up until just a few days ago, I could never get anything to work. After working on getting a PPTP-based VPN working from my [PowerBook](http://www.apple.com/powerbook/), I realized that just as I had to modify my ipfw rules (using BrickHouse) to allow the PPTP traffic, I'd have to modify the rules to allow IPSec traffic as well. Duh!

After making the changes to allow ISAKMP (UDP port 500), ESP (IP protocol 50), and AH (IP protocol 51), I created a quick-and-dirty IP Security policy on the Windows box. Lo and behold, it looked as if an SA was established. However, within just a few seconds after IPSecuritas' log showed the SA established, then it appeared as if `racoon` (the back-end for IPSec on Mac OS X) crashed. I have not yet figured out why.

Even with the apparent `racoon` crash, it looked like the SA was still valid. I was not able to verify encryption by sniffing the traffic, but it certainly seemed like everything was working.

Despite the problems thus far, this is still good news to me. Here's hoping I can figure the rest of the problem out.
