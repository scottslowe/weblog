---
author: slowe
categories: Rant
comments: true
date: 2016-03-17T00:00:00Z
tags:
- OpenStack
- OSS
- Linux
- Virtualization
- AWS
- VMware
- vSphere
title: On the Topic of Lock-In
url: /2016/03/17/on-topic-of-lock-in/
---

While talking with customers over the past couple of weeks during a multi-country/multi-continent trip, one phrase that kept coming up is "lock-in", as in "we're trying to avoid lock-in" or "this approach doesn't have any lock-in". While I'm not a huge fan of memes, this phrase always brings to mind _The Princess Bride_, Vizzini's use of the word "inconceivable," and Inigo Montoya's famous response. C'mon, you know you want to say it: "You keep using that word. I do not think it means what you think it means." I feel the same way about lock-in, and here's why.

Lock-in, as I understand how it's viewed, is an inability to migrate from your current solution to some other solution. For example, you might feel "locked in" to Microsoft (via Office or Windows) or "locked in" to Oracle (via their database platform or applications), or even "locked in" to VMware through vCenter and vSphere. Although these solutions/platforms/products might be the right fit for your particular problem/need, the fact that you can't migrate is a problem. Here you are, running a product or solution or platform that is the right fit for your needs, but because you may not be able to migrate to some other platform at some point in the future you're "locked in." Therefore, in order to keep your options open, you feel like you have to choose a different solution, perhaps even settling for a solution that is less ideally suited to solve the problem(s) you're trying to solve.

Based on this understanding, the key question that comes to my mind is this: what makes you think you can avoid lock-in?

The reality is that _every single platform/solution/product out there has lock-in._ They might have various levels of lock-in, but lock-in exists everywhere:

* Linux has lock-in. (Gasp!) Don't believe me? Ever run into a problem running a script because it contained idiosyncrasies specific to a particular distribution? Yes, these situations can be minimized, but the fact remains that there is some level of lock-in present.
* KVM has lock-in. (Gasp!) You can't run QCOW2 disk images on another hypervisor without conversion. Yes, conversion tools are available---but so are conversion tools for vSphere and Hyper-V.
* Docker has lock-in. (Gasp!) Can you take a Docker container and run it using LXC? Yes, work is progressing on this front, but we're not there yet, and may never be.
* OpenStack has lock-in. (Gasp!) Try writing some Heat templates in HOT format and then see if they'll work with a cloud platform other than OpenStack.
* Public cloud providers have lock-in. (Gasp!) Let's face it, once you start taking advantage of services or APIs that are specific to a provider, you're locked in. Or there's the concept of being locked in because it's _too expensive_ to get your data out (think multiple terabytes of data in Amazon S3).

The thing that gets me is when people place avoiding lock-in (which is impossible) above other, potentially more important, design considerations. Let's take operational costs as an example. Let's assume, just for the purposes of discussion, that using KVM on Linux offers less lock-in than using VMware vSphere. Does it offer less operational cost? Is it easier and cheaper to operate? What about the staff training that would be required to operate it? What about backups? What about monitoring? You can't ignore these design considerations in favor of trying to avoid the unavoidable. 

The point is not to try to avoid lock-in---because you can't---but to recognize that "lock-in" is simply another variable to be considered in the overall design and architecture of your system. As a variable to be considered in design, you can then place an appropriate emphasis on minimizing lock-in versus solving business applications versus operational costs versus the cost of future migrations versus...well, you get the idea.

Let's stop placing the avoidance of "lock in" above other equally important design considerations and instead focus on finding the _best_ solution for the problem at hand.
