---
author: slowe
categories: Explanation
comments: false
date: 2006-01-04T17:28:35Z
slug: mac-os-x-and-local-domains
tags:
- ActiveDirectory
- Apple
- Bonjour
- Macintosh
- Microsoft
- Networking
- Windows
- Interoperability
title: Mac OS X and .local Domains
url: /2006/01/04/mac-os-x-and-local-domains/
wordpress_id: 150
---

Some time ago, [Mac OS X Hints](http://www.macosxhints.com/) published a hint I submitted regarding the [use of the `.local` TLD](http://www.macosxhints.com/article.php?story=20040806232315819) (top level domain) with Mac OS X. Specifically, the hint centered around the use of Mac OS X with Active Directory domains using the `.local` TLD. For ease of access, here's that same hint.

Basically, Mac OS X uses the `.local` TLD for Bonjour/Rendezvous services, and is configured to use [multicast DNS (mDNS)](http://www.multicastdns.org/) for discovery of those services. This configuration occurs via a file named `local` in the `/etc/resolver` directory. Apple's Knowledge Base article offers a solution, but that solution involves editing this `local` file, which affects Bonjour/Rendezvous operation. This solution, on the other hand, does not affect the `local` file in any way, and thus does not interfere with Bonjour/Rendezvous.

Let's say that you need to integrate Mac OS X with an Active Directory domain called _company.local._ Simply create a file in `/etc/resolver` named `company.local` with the following contents:

    nameserver a.b.c.d
    nameserver w.x.y.z
    port 53

Obviously, replace the letters in the text above with the IP addresses of your appropriate DNS servers for the company.local Active Directory domain. Then, flush the lookupd cache with `lookupd -flushcache` and that's it!

With this file in place, your Mac OS X system will resolve _company.local_ (or _subdomain.company.local_) via the instructions in the file `/etc/resolver/company.local`, but will handle Bonjour/Rendezvous service discovery via mDNS in the same fashion.
