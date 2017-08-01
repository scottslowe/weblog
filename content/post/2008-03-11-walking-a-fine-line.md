---
author: slowe
categories: Musing
comments: true
date: 2008-03-11T10:15:35Z
slug: walking-a-fine-line
tags:
- Security
- Storage
- Virtualization
- VMware
title: Walking a Fine Line
url: /2008/03/11/walking-a-fine-line/
wordpress_id: 655
---

A [recent post](http://dcsblog.burtongroup.com/data_center_strategies/2008/03/vmworld-europ-2.html) by Chris Wolf over at Data Center Strategies has highlighted the dangerous position of any market leader, although this post specifically discusses VMware and their leadership position in the x86 virtualization market.

The post discusses some of the backlash from storage vendors in response to the release of Storage VMotion by VMware. Storage VMotion, as you are probably already aware, offers VMware users the ability to move the storage for running virtual machines from one storage location to another storage location without downtime. It's a pretty useful function, although the user interface [isn't too great][1]. Apparently, the addition of this functionality to the VMware product offering has alienated some vendors who offered this feature within their own storage arrays. My personal view is that the vendors' offering is probably faster, more robust, and more feature-rich than Storage VMotion, at least for now, and therefore storage vendors shouldn't be so "up in arms" over this issue.

But this issue highlights the fine line that a market leader such as VMware has to walk. If they don't add functionality to their products, then they lose the "innovation leadership" position in their market and run the risk of being overtaken by their competitors. After all, VMware needs to continue to distinguish itself from other virtualization competitors such as Citrix, Microsoft, and Virtual Iron. How else to distinguish themselves other than adding functionality that helps their customers improve their uptime?

On the other hand, as the market leader---VMware, in this case---adds functionality and features to their products, they run the ever-increasing risk that they are going to step on the toes of some of their partners. Consider the [Determina acquisition][2], which has yet to yield publicly disclosed results. What's going to happen when VMware unveils technology based on the Determina acquisition? They will be accused of isolating the security partners in the virtualization ecosystem. And yet, isn't it natural that they would want to incorporate those security features within their own products?

It's a difficult position. If VMware---or the market leader in other markets, like Microsoft with Windows in the OS market---strays too far one way or the other, they are accused of either a) failing to meet the needs of the customer for failing to add critical functionality; or b) being too aggressive in bundling functionality into the base product.

I certainly don't envy VMware in their current position. With Citrix, Microsoft, and Virtual Iron all nipping at their heels, their [stock taking a hit](http://www.virtualization.info/2008/03/vmw-stock-price-is-back-at-starting.html), and partner vendors complaining about the functionality they're bundling into their products, it seems as if it's coming from all sides.

[1]: {{< relref "2008-01-07-underwhelmed-by-the-remote-cli.md" >}}
[2]: {{< relref "2007-08-18-vmware-addressing-virtual-security-concerns.md" >}}
