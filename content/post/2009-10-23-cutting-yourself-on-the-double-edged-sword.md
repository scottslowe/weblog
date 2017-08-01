---
author: slowe
categories: Rant
comments: true
date: 2009-10-23T09:45:32Z
slug: cutting-yourself-on-the-double-edged-sword
tags:
- Cisco
- FCoE
- Networking
- Virtualization
- Xsigo
title: Cutting Yourself on the Double-Edged Sword
url: /2009/10/23/cutting-yourself-on-the-double-edged-sword/
wordpress_id: 1702
---

Yesterday I published a short post titled ["I/O Virtualization and the Double-Edged Sword"][1]. In that post, I discussed how Xsigo was criticizing FCoE for "not going far enough" in the realm of I/O virtualization. Unfortunately, I didn't do a very good job of really getting my point across, because the discussion rapidly turned into a discussion of the merits of various interconnect technologies and why one might win over the other. While that is a great discussion to have---and I'm thrilled my site can help further that discussion---it wasn't really the key point behind my article. I/O virtualization was only the catalyst to prompt the original post.

Let me see if I can more clearly articulate what I'm trying to say here. If you are a [Twitter](http://twitter.com) user and into virtualization or storage, then you probably are following either Chad Sakac of EMC ([@sakacc](http://twitter.com/sakacc) on Twitter), Vaughn Stewart of NetApp ([@vaughn_stewart](http://twitter.com/vaughn_stewart) on Twitter), or both. That being the case, you are probably very familiar with the extensive "discussions" that take place between the two of them. Both of them are very passionate about storage and virtualization, but they have differing viewpoints. Now, before I'm accused by NetApp of being an EMC bigot (which would be ridiculous given the coverage I've given NetApp) or accused by EMC of being a NetApp bigot (that, at least, might be understandable as I'm just now starting to learn EMC storage), let me say that I'm not endorsing either product. NetApp's products and EMC's products are different; each of them has strengths and weaknesses in different areas.

Now, ask yourself, "Why do these products have different strengths and weaknesses?" Do you know the answer? **These products have different strengths and weaknesses because of the technology decisions each company chose to make in the products' development.** NetApp chose one path, EMC chose another. For NetApp, that has created certain efficiences, certain strengths---and corresponding weaknesses. Likewise, EMC's technology decisions have resulted in their products having certain strengths and weaknesses. Neither of these products is perfect. For NetApp to claim that "their way is the right way" is ridiculous; their way is only one of many different ways to accomplish something. The same is true for EMC. And, by extension, the same is true for _every other technology vendor on the planet._

You want more examples? Consider the architectural differences between VMware ESX/ESXi and Microsoft Hyper-V. The technology choices made by each company created inherent strengths and weaknesses in each product. VMware claims their choices are the best choices; Microsoft believes their architecture is the best. Clearly, neither product is perfect. Both products have their flaws.

The real key takeaway here is that _no technology vendor has the right to throw rocks at another technology vendor._ All technology vendors live in glass houses. For VMware to claim that Microsoft's architecture is all wrong is, well, wrong. For EMC to say that NetApp's technology choices are stupid would be wrong. For Xsigo to claim that FCoE is the wrong path for I/O virtualization is wrong (although, personally, I don't consider FCoE an I/O virtualization technology, but that's a different discussion for a different day). Why? Because every company has to make technology choices, and those technology choices will---by the very nature of technology---automatically create inherent differences, strengths, and weaknesses in the resulting product. And when you accept that truth (and it **is** a truth, I promise you), then you see why vendors should not engage in negative marketing. When a vendor engages in negative marketing about the competition, that vendor is simply inviting others to pick apart the flaws in their own products.

Of course, I'm not naive enough to believe that vendors will stop negative competitive marketing overnight. Still, I stand firm in the belief that those vendors that focus on the strengths of their products instead of the flaws of others' products will move ahead. I'm certainly more likely to do business with them.

I'd be interested to hear what others have to say. Voice your position in the comments.

_Disclosure: As you probably know, I work for a reseller who represents many different vendors and manufacturers. My words here are not endorsed by my employer, nor do I represent my employer in this area._

[1]: {{< relref "2009-10-22-io-virtualization-and-the-double-edged-sword.md" >}}
