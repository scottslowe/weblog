---
author: slowe
categories: News
comments: true
date: 2008-08-21T22:21:49Z
slug: vmsight-in-vdi-environments
tags:
- Security
- VDI
- Virtualization
- VMware
title: vmSight in VDI Environments
url: /2008/08/21/vmsight-in-vdi-environments/
wordpress_id: 837
---

So I've been talking with some guys from [vmSight](http://www.vmsight.com/) recently in an effort to better understand their products and where their products fit into an overall solution. Just when I thought I had a handle on their feature set and how it can be used, yesterday they [introduced User Activity Control for VDI](http://www.vmsight.com/pr_08202008.asp).

User Activity Control for VDI changes the nature of vmSight's solution. This shifts vmSight from a primarily passive solution, watching network traffic and using their Connector ID technology to intelligently calculate application response times and network latency, to a more active solution capable of blocking unauthorized network activity. In my mind, this now gives vmSight three key features that organizations with virtualized infrastructures may find useful:

1. The ability to gather network traffic information and use that, along with the Connector ID technology, to calculate application response times and network latencies in order to establish or maintain service level agreements (SLAs)

2. The ability, based on network activity, to identify VMs that are not actively being used so that those VMs can be reclaimed and resources used elsewhere, helping to reduce VM sprawl

3. Active traffic blocking functionality to enforce security policies and prevent unauthorized traffic or access, new through User Activity Control for VDI

I haven't yet had the chance to actually get my hands on the technology, but it looks pretty useful. It would be great to hear back from any actual vmSight users to get their feedback on the solution and how well it works for their organizations.
