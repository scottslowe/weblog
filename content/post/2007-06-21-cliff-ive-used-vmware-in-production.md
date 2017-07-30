---
author: slowe
categories: Rant
comments: true
date: 2007-06-21T23:55:26Z
slug: cliff-ive-used-vmware-in-production
tags:
- Virtualization
- VMware
title: Cliff, I've Used VMware in Production
url: /2007/06/21/cliff-ive-used-vmware-in-production/
wordpress_id: 475
---

"How many people have deployed VMware or Xen or even Microsoft Virtual Server in a real production environment?" That's the question Cliff Saran's IT FUD blog [asked yesterday](http://www.computerweekly.com/blogs/IT-FUD-blog/2007/06/does-virtual-make-sense.html). It's an interesting question to ask an engineer such as myself who specializes in VMware deployments for customers. And while I can't give out the names of some of my customers, I can tell you that more than a couple of them are using [VMware Virtual Infrastructure 3](http://www.vmware.com/products/vi/) in production environments _right now._

Here are some examples of how these customers are using VMware:

* One customer has over 20 dual-processor server blades (older HP p-Class blades, by the way---trying to get them to transition to the newer c-Class blades) running [ESX Server 3.0.1](http://www.vmware.com/products/vi/esx/) in a big DRS cluster hosting over 120 virtual servers. These virtual servers are application servers, middleware servers, Exchange front-end servers, and Citrix Presentation Servers, to name a few.

* Another customer, still early in their VI3 deployment, has a three-node DRS/HA cluster running a variety of workloads, such as [Microsoft Office Project Server](http://office.microsoft.com/en-us/projectserver/default.aspx) and a couple other application servers, on VMware ESX Server.

* One very small customer I have is using [Virtual Server](http://www.microsoft.com/windowsserversystem/virtualserver/) to host a few workloads, including a middleware server for a web-based application with an SQL backend.

And these are just the examples that I know about. What about all the other VMware engineers in my company, not to mention all the other VARs with strong VMware practices? And Cliff says that he's having a hard time finding references? That doesn't make sense to me.

While I love virtualization (and VMware products in particular), I'll be the first to admit that virtualization is not the "be all/end all" that some make it out to be. Is it useful in many organizations? Yes, absolutely. Will it fit in every situation? No, it won't. There _will_ be some situations where virtualization is not the answer. However, those situations are fairly limited, and growing more limited by the day.

What about you? Cliff wants stories of people using VMware in production environments, so let's give 'em to him. Tell us about your production VMware environment below in the comments.
