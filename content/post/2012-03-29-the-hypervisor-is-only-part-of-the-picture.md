---
author: slowe
categories: Rant
comments: true
date: 2012-03-29T09:00:00Z
slug: the-hypervisor-is-only-part-of-the-picture
tags:
- ESX
- ESXi
- Virtualization
- VMware
- vSphere
title: The Hypervisor is Only Part of the Picture
url: /2012/03/29/the-hypervisor-is-only-part-of-the-picture/
wordpress_id: 2570
---

The question of VMware's future in the face of increasing competition is not a new one; it's been batted around by quite a few folks. So Steven J. Vaughan-Nichols' article ["Does VMware Have a Real Future?"](http://www.pcworld.com/businesscenter/article/252579/does_vmware_have_a_real_future.html) doesn't really open any new doors or expose any new secrets that haven't already been discussed elsewhere. What it _does_ do, in my opinion, is show that the broader market hasn't yet fully digested VMware's long-term strategy.

Before I continue, though, I must provide disclosure: what I'm writing is _my_ interpretation of VMware's strategy. Paul M. hasn't come down and given me the low-down on their plans, so I can only speculate based on my understanding of their products and their sales strategy.

Mr. Vaughan-Nichols' article focuses on what has been, to date, VMware's most successful technology and product: the hypervisor. Based on what I know and what I've seen in my own experience, VMware created the virtualization market with their products and cemented their leadership in that market with VMware Infrastructure 3 and, later, vSphere 4 and vSphere 5. Their hypervisor is powerful, robust, scalable, and feature-rich. Yet, the hypervisor is only one part of VMware's overall strategy.

If you go back and look at the presentations that VMware has given at VMworld over the last few years, you'll see VMware focusing on what many of us refer to as the "three layer cake":

1. Infrastructure
2. Applications (or platforms)
3. End-user computing

If you like to think in terms of *aaS, you might think of the first one as Infrastructure as a Service (IaaS) and the second one as Platform as a Service (PaaS) or Software as a Service (SaaS). Sorry, I don't have a *aaS acronym for the third one.

I believe that VMware knows that relying on the hypervisor as its sole differentiating factor will come to end. We can debate how quickly that will happen or which competitors will be most effective in making that happen, but those issues are beside the point. This is not to say that VMware is ceding the infrastructure/IaaS market, but instead recognizing that it cannot be all that VMware is. VMware must be _more_.

What is that "more"? I'm glad you asked.

Let's look back at the forces that drove VMware's hypervisor into power. We had servers with more capacity than the operating system (OS)/application stack could effectively leverage, leaving us with lots of servers that were lightly utilized. We had software stacks that drove us to a "one OS/one application" model, again leading to lots of servers that were lightly utilized. Along comes VMware with ESX (and later ESXi) and the ability to fix that problem, and---this is a key point---fix it without sacrificing compatibility. That is, you could continue to deploy your OS instances and your application stacks in much the same way. No application rewrite needed. That was incredibly powerful, and the market responded accordingly.

This compatibility-centered approach is both powerful yet limiting. Yes, you can maintain status quo, but the problem is that you're maintaining status quo. Things aren't really changing. You're still bound by the same limitations as before. You can't really take advantage the new functionality the hypervisor has introduced.

Hence, applications need to be rewritten. If you want to really take advantage of virtualization, you need a---gasp!--platform designed to exploit virtualization and the hypervisor. This explains VMware's drive into the application development space with vFabric (Spring, GemFire, SQLFire, RabbitMQ). These tools give them the platform upon which a new generation of applications can be built. (And I haven't even yet touched on CloudFoundry.) This new generation of applications will _assume_ the presence of a hypervisor, and be able to exploit the functionality provided by it. However, a new generation applications that are still bound by the old ways of accessing those applications will constrain their effectiveness.

Hence, end users need new ways to access these applications, and organizations need new ways to deliver applications to end users. This explains VMware's third layer in the "three layer cake": end-user computing. Reshaping applications to embrace new form factors (tablets, smartphones) means re-architecting your applications. If you're going to re-architect your applications, you might as well build them using a using a new platform and set of tools that lets you exploit the ever-ubiquitous presence of a hypervisor. Starting to see the picture now?

If you look at VMware only from the perspective of the hypervisor, then the question of VMware's future viability is suspect. I'll grant that. Take a broader look, though---look at VMware's total vision and I think you'll see a different picture. That's why---assuming VMware can execute on this vision---I think that the answer to Mr. Vaughan-Nichols' question, "Does VMware have a real future?", is yes. VMware might not continue to reign as an undisputed market leader, but I do think their long-term viability isn't in question (again, assuming they can execute on their vision.)

Feel free to share your thoughts in the comments. Do _you_ think VMware has a future? What should they do (or not do) to ensure future success? Or is their fall a foregone conclusion? I'd love to hear your thoughts. I only ask for disclosure of vendor affiliations, where applicable. (For example, although I work for EMC and EMC has a financial relationship with VMware, I speak only for myself.)
