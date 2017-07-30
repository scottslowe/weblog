---
author: slowe
categories: Rant
comments: true
date: 2009-10-22T14:19:27Z
slug: io-virtualization-and-the-double-edged-sword
tags:
- Cisco
- FCoE
- Networking
- Virtualization
- Xsigo
title: I/O Virtualization and the Double-Edged Sword
url: /2009/10/22/io-virtualization-and-the-double-edged-sword/
wordpress_id: 1698
---

I recently came across [this blog entry](http://www.xsigo.com/blog/?p=48) over at Xsigo's new corporate blog, [I/O Unplugged](http://www.xsigo.com/blog/). A key phrase in this blog entry really caught my eye:

>The reality is that FCoE solves neither the complexity nor the management problems. It is a minor change to the status quo when a major leap forward comparable to server virtualization is needed for I/O.

At first glance, I'd say they are right. FCoE was designed from the ground up to be completely compatible with Fibre Channel---and _that's one of its key strengths._ Yes, Xsigo's InfiniBand-based solution is a very different architecture, and the set of capabilities provided by the Xsigo I/O Director are very different than the capabilities enabled by an FCoE solution such as the Cisco Nexus 5000 (or the newly-announced Nexus 4000). I wouldn't necessarily disagree that Xsigo's solution might offer some benefits over FCoE. I would strongly contend, however, that FCoE does offer some benefits over InfiniBand-based I/O virtualization solutions.

See, every technology decision is a double-edged sword. Xsigo "breaks the mold" by using a new architecture based on InfiniBand, but this decision comes at the cost of compatibility. Cisco chooses to go with a "less innovative" solution, but gains the benefit of broad compatibility with a large installed base. There is no one solution that offers all advantages and no disadvantages. That being said, which is more important to you and your company: innovation or compatibility? These are the sorts of questions you need to ask when evaluating solutions.

What do you think? Feel free to post your thoughts below. Vendors, please be sure to disclose your affiliation. And, in the spirit of full disclosure, keep in mind that my employer is a Cisco partner, but I have worked with both Xsigo and Cisco solutions. The thoughts I post here do not reflect the thoughts or views of my employer.
