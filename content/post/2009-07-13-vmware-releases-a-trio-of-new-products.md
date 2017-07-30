---
author: slowe
categories: News
comments: true
date: 2009-07-13T09:08:38Z
slug: vmware-releases-a-trio-of-new-products
tags:
- Virtualization
- VMware
title: VMware Releases a Trio of New Products
url: /2009/07/13/vmware-releases-a-trio-of-new-products/
wordpress_id: 1461
---

Today VMware is releasing three new products in the vCenter line:

* vCenter Lab Manager 4.0, the next major version of VMware's very popular lab automation software;

* vCenter AppSpeed; and

* vCenter Chargeback.

vCenter Lab Manager 4.0 brings a few very significant features, not the least of which is full vSphere compatibility. Another major feature is cross-host VM fencing, which allows users to leverage Lab Manager's fencing functionality---thus allowing multiple teams of networks to share the exact same TCP/IP configuration---while also taking advantage of VMotion and Distributed Resource Scheduler (DRS). This is a big one, as I've had multiple customers inquire about the interaction between Lab Manager and other features like DRS.

With the release of Lab Manager 4.0, VMware is also taking the step of integrating some products with overlapping functionality. I've had more than a few questions about the relationship between vCenter Lab Manager and vCenter Stage Manager. Apparently, that was a question lots of people had, and to answer that question VMware is now integrating Stage Manager and Lab Manager into a single product. This makes a lot of sense, in my opinion, and will definitely help strengthen the Lab Manager product.

Also being released today is vCenter AppSpeed, VMware's performance management product. Based on the technology acquired from Beehive, AppSpeed aims to give VMware administrators the ability to "see" deeper into the environment than before. With that visibility, then, comes the ability to more accurately pinpoint current and potential performance bottlenecks, as well as the ability to prove end-user performance metrics. I'm sure you've run into this problem: you virtualize an application, and then an end user complains about performance. Using AppSpeed, VMware administrators can determine if the virtualization layer is what caused a performance impact, or if there is a performance impact at all.

Finally, vCenter Chargeback is the third product being released. Lots of customers talk about chargeback, but very few customers actually implement chargeback. VMware hopes to make chargeback easier with its new product, which allows customers to start with "show-back", i.e., showing the business what it costs for each virtual machine being provisioned. This also has the (perhaps unintended) side effect of helping to curb VM sprawl, which is usually caused by a lack of understanding of the cost of resources within the virtualized environment.

For more information on any of these products, visit [VMware's web site](http://www.vmware.com). (As of the time I published this post, VMware's web site had not yet been updated with the new information.)
