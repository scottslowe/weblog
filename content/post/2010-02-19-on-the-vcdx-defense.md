---
author: slowe
categories: Rant
comments: true
date: 2010-02-19T22:06:36Z
slug: on-the-vcdx-defense
tags:
- Virtualization
- VMware
- vSphere
title: On the VCDX Defense
url: /2010/02/19/on-the-vcdx-defense/
wordpress_id: 1846
---

I've been thinking about how to write this post since last Friday afternoon after I completed my VCDX defense panel. Even now, a week later, I'm not sure that I have the right words to use.

After many long hours of preparation on the application and the submitted design, after days and weeks of waiting, after hours spent reviewing the design, and after reading numerous tips and tricks from established VCDXs, it all culminated in the defense panel. There, in the defense panel, I would have to stand before three knowledgeable, established design experts and defend the choices made in the design. I'd have to explain why I chose block storage over NFS, why Fibre Channel over iSCSI, why blade servers vs. rack-mount servers. I'd have to explain why I chose the LUN size I chose, and I'd have to defend the zoning that controlled the presentation of those LUNs. Clusters, cluster sizes, features enabled or not enabled, networking layout, VM density on the LUNs, projected IOPS---nothing was safe from their inquisition. And yes, I'd have to explain why the design used NetApp storage instead of EMC storage. (It was a customer requirement, i.e., a design constraint.)

Surprisingly, when the time for the VCDX defense panel arrived I found myself a lot more nervous than I had expected I would be. After all, this was just a friendly conversation with technical peers, right? In many ways it could be viewed that way, but the underlying purpose behind the conversation was ever-present: there was a reason I was there standing before these three people. It wasn't just "shooting the breeze" with friends; there was a purpose there. It wasn't just bouncing ideas off co-workers or industry colleagues; there was a reason for the conversation. It's not that the panelists did anything to cause this feeling; they were completely fine, very courteous and quite friendly. (In case you're wondering, I'm not going to disclose who was on my panel. They are welcome to disclose if they so choose, but that will be their decision.)

Looking back on it now, I realize that I should have gotten better control of my nerves. I spoke too quickly. I rushed through questions that probably deserved more explanation. I forgot details about my design. I got tripped up by relatively simple questions. I've made no secret of the fact that I wasn't pleased by how well I performed---or didn't perform---in the VCDX defense panel. I was upset that I had been thrown off and that I wasn't able to recall all the details from my design. For a few hours after completing the defense panel, I beat myself up over how things had gone. But it didn't take me too long to make peace with not having passed. I knew that if I had not passed, the experience was still worthwhile as a learning experience. Even if I hadn't gained the VCDX certification, I'd still gained knowledge and experience. And hey, there was always another chance to defend at VMworld, right?

After returning home from Las Vegas, I spent the week thinking about what I would write after I'd finally gotten my results. I tried to prepare for the questions like "How in the world could Scott not pass?" I thought about explaining that the defense panel was only doing their job; they were preserving the value of the certification. After all, if the bar is not held high, what is the value of VCDX? All the while I secretly hoped that the result would be something other than what I was confident it would be.

And so it was that as I was driving my kids to a church youth group function tonight---after a long and unproductive day working with some rather stubborn equipment in the lab---that I received an e-mail from VMware. The first line of the message was this:

>Congratulations! You have achieved the VCDX3 certification. Your VCDX number is: VCDX39

Unbelievable! I'd passed! I was so excited. I'd hoped for this result, but I honestly did not believe that I had managed to pull it off. I immediately called Crystal to tell her the news. I think she might have been even more excited than me.

Having now been through this entire process, what advice do I have for aspiring VCDX candidates?

* As many others have stated, _know your design._ If I had only one thing to change about my entire process, this would be it. You should know it forward and backward: every detail, every choice, and every reason behind the design.

* Don't be too nervous. I allowed my nerves to get the best of me, there's no question. I also don't doubt that I would have done better had I not been so nervous. (As a side note, it's interesting to me that I can stand up and speak in front of large crowds and not be nervous, but standing in front of those panelists really threw me. Odd.)

* Understand _the impact of your choices_. As Duncan pointed out in this recent blog post, it's really [about the impact](http://www.yellow-bricks.com/2010/02/15/impact-of-decisions/). Be prepared to discuss the reasons for the decisions in your design and the impact of the decisions in your design.

That's it from the latest VCDX to join the ranks. I'll post another update later with more tips and tricks that I learned from the experience, but those are some that jump to my mind immediately.

Have a great weekend!
