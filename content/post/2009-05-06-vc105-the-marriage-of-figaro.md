---
author: slowe
categories: Liveblog
comments: true
date: 2009-05-06T16:41:58Z
slug: vc105-the-marriage-of-figaro
tags:
- Security
- Virtualization
title: 'VC105: The Marriage of Figaro'
url: /2009/05/06/vc105-the-marriage-of-figaro/
wordpress_id: 1320
---

This is Christofer Hoff's session at Virtualization Congress. The title of the session is "The Marriage of Figaro: Complexity and Insecurity of the Cloud". I'm looking forward to the presentation, as I've heard some great things about Christofer's presentations. Unfortunately, I can't get a Internet connection here in the break-out session room, so this will have to be published later. (Even if I did get a wireless connection, the network here at Synergy seems to be incapable of supporting the demands placed upon it.)

The story behind the title of the presentation is an allusion to the comment made about Mozart's The Marriage of Figaro that "it had too many notes." Hoff thinks that this is particularly applicable to cloud computing. However, after exploring the facts behind his theme, Hoff realized that this just wasn't the right theme, so he declared thematic fail and transitioned the presentation over to "The Frogs Who Desired a King," based on Aesop's fable about frogs who wished to have a king.

Hoff gets into cutting through the hype and the FUD about cloud computing and what is real: abstraction of virtualization, resource democratization, and service orientation. However, these things are things that have been around for a while. What's new in cloud computing is elasticity/dynamism and a utility model (consumption/allocation). This differentiates cloud computing from previous computing trends.

What makes things "cloud-ready"? Some attributes would include:

* Processes, applications, and data are largely independent
* Points of integration are well-defined
* High level of security required
* Core internal architecture needs work

With that in mind, there are really only three archetypes of cloud computing:

* IaaS
* PaaS
* SaaS

Hoff then goes on to compare the various components of SaaS/PaaS/IaaS (which he refers to as SPI) to a seven layer dip, then goes deeper into the actual models and interaction of the components within these different types of cloud computing. This shows how PaaS simply builds on top of IaaS by adding another layer of integration and middleware on top of the IaaS APIs. Similarly, SaaS builds on top of PaaS (and IaaS) by adding data, metadata, applications, APIs, presentation platforms, and presentation mobility.

Looking at these archetypal models in a dimensional model you can see how SaaS may have higher levels of security, but lower levels of extensibility. Conversely, IaaS will have higher levels of extensibility but lower levels of security. That means that the lower in the SPI stack that the provider stops, the more liable you---the end user---are for ensuring security. This doesn't mean a reduction of risk, but simply a transfer of risk.

That means you can't answer the question, "Is the cloud more secure?" can't be answered without context.

Hoff next moves into a discussion of hosted services versus cloud services. What are the differences? Underneath the covers, the differences are in single tenancy vs. multi-tenancy, isolated data vs. co-mingled data, and dedicated secuirty vs. social security.

Let's apply all these concepts against the journey of a large enterprise organization toward cloud computing. The first phase is virtualization to achieve consolidation. The second phase is supposed to be automation and optimization, but it is "really freaking hard" (RFH). As a result, most organizations have skipped Phase 2 and moved to Phase 3, which is essentially embracing cloud computing.

What about private clouds vs. public clouds? Hoff discusses the various definitions of public clouds and private clouds. To Hoff, having a private cloud is more than just adding chargeback to your virtualized infrastructure.

The Jericho Forum's Cloud Cube model allows organizations to define cloud computing on a number of axes: internal/external, proprietary/open, perimeterized/de-perimeterized, and outsourced/insourced.

According to Hoff, we've rushed to embrace virtualization without resolving issues like virtualization management, we've brushed past the automation and self-service business processes that would have added maturity to virtualization, and are now rushing to cloud computing. How can something _not_ go wrong? This leads to simplexity (simplest representation of complexity) and the "squeezing the balloon" problem. Issues haven't been solved, they've just been shifted.

What's true with VirtSec (virtualization security) is even more true with CloudSec (cloud security). Depending upon the type of cloud service, you may not get feature parity for security. Your visibility and ability to deploy compensating controls are greatly diminished or even eliminated. Many of the things we do today are shifting controls away from the network back into the host or the guest, where that's even possible.

Hoff shows how computing has evolved, but the answer to security problems has remained the same over almost twenty years. The answer to security problems remains firewalls and SSL, but these technologies simply do not address today's security concerns.

The "Hamster Sine Wave of Pain" shows how security cyclically moves from network centricity to application centricity to information centricity to user centricity to host centricity and then back again. Yet at each and every one of these steps we have still failed to address the fundamental security issues.

Hoff describes a number of "new security threats" pertaining to cloud computing, like CloudFlux (turning up virtual botnets via Amazon EC2), LeapFrog (using and abusing VPNs between clouds), or EDoS (economic denial of service). That last option (EDoS) describes a scenario in which a competitor drives up utilization (and thus drives up the pay-as-you-go bill) and forces a company out of business.

Interesting port: Amazon EC2's terms of service forbid vulnerability assessments or pen testing.

Wrapping up and bringing it back to the fable upon which the presentation is based, the fable is that we are screwed with regard to security. The reality is that we are not, but instead we are just as insecure as we've always been. This goes back the "squeezing the balloon" problem---the security problems have just been shifted elsewhere.
