---
author: slowe
categories: Information
comments: true
date: 2014-04-22T12:35:22Z
slug: kicking-the-policy-discussion-into-high-gear
tags:
- Interoperability
- OpenStack
- OSS
- Policy
- Cloud
title: Kicking the Policy Discussion Into High Gear
url: /2014/04/22/kicking-the-policy-discussion-into-high-gear/
wordpress_id: 3433
---

Most IT vendors agree that more extensive use of automation and orchestration in today's data centers are beneficial to customers. The vendors may vary in their approach to providing this automation and orchestration---some may prefer to do it in software (VMware would be one of these, along with other software companies like Microsoft and Red Hat), while others want to do it in hardware. There are advantages and disadvantages to each approach, naturally, and customers need to evaluate the various solutions against their own requirements to find the best fit.

However, the oft-overlooked problem that more extensive use of automation and orchestration creates is one of _control_---specifically, how customers can control this automation and orchestration according to their own specific policy. A [recent post](http://networkheresy.com/) on [the Network Heresy site](http://networkheresy.com/) discusses the need for policy in fully automated IT environments:

>However, fully automated IT management is a double-edged sword. While having people on the critical path for IT management was time-consuming, it provided an opportunity to ensure that those resources were managed sensibly and in a way that was consistent with how the business said they ought to be managed. In other words, having people on the critical path enabled IT resources to be managed according to _business policy._ We cannot simply remove those people without also adding a way of ensuring that IT resources obey business policy---without introducing a way of ensuring that IT resources retain the same level of policy compliance.

VMware, along with a number of other companies, has launched an open source effort to address this challenge: finding a way to enable customers to manage their resources according to their business policy, and do so in a cloud-agnostic way. This effort is called Congress, and it has received [some attention from those who think it's a critical project](http://sarob.com/2014/03/my-take-on-openstack-projects-congress-part-1-of-10/)). I'm really excited to be involved in this project, and I'm also equally excited to be working with some extremely well-respected individuals across a number of different companies (this is most definitely **not** a VMware-only project). I believe that creating an open source solution to the policy problem will further the cause of cloud computing and the transformation of our industry. I strongly urge you to read [this first post, titled "On Policy in the Data Center: The policy problem"](http://networkheresy.com/2014/04/22/on-policy-in-the-data-center-the-policy-problem/), and stay tuned for future blog posts that will dive into even greater detail. Exciting times are ahead!
