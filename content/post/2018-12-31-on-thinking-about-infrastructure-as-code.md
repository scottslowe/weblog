---
author: slowe
categories: Musing
comments: true
date: 2018-12-31T11:00:00Z
tags:
- Automation
- Development
title: On Thinking About Infrastructure as Code
url: /2018/12/31/on-thinking-about-infrastructure-as-code/
---

I just finished reading Cindy Sridharan's excellent post titled "[Effective Mental Models for Code and Systems][link-1]," and some of the points Sridharan makes immediately jumped out to me---not for "traditional" code development, but for the development of infrastructure as code. Take a few minutes to go read the post---seriously, it's _really_ good. Done reading it? Good, now we can proceed.<!--more-->

Some of these thoughts I was going to share in a planned presentation at Interop ITX in May 2019, but since I'm unable to speak at the conference this year due to schedule conflicts (my son's graduation from college and a major anniversary trip for me and Crystal), I figured now was as good a time as any, especially given the timing of Sridharan's post. Also, a lot of these thoughts stem from a discussion with a colleague at work, which in turn led to [this Full Stack Journey podcast on practical infrastructure as code][link-2].

Anyway, let me get back to Sridharan's post. One of the things that jumped out to me right away was Sridharan's proposed hierarchy of needs for code:

![Sridharan's hierarcy of needs for code](/public/img/sridharan-hierarchy-needs.png)

As you can see in the image (full credit for which belongs to Sridharan, as far as I know), making code _understandable_ lies at the bottom of the hierarchy of needs, meaning it is the most basic necessity needed. Until this need is satisfied, you can't move on to the other needs. Sridharan puts it this way:

>Optimizing for understandability can result in optimizing for everything else on the hierarchy depicted above.

Many readers have probably heard of [the DRY principle][link-3] when it comes to writing code. (DRY stands for Don't Repeat Yourself.) In many of the examples of infrastructure as code that I see online, the authors of these examples tend to use control structures such as Terraform's `count` construct when creating multiple infrastructure objects. I'll use some code that I wrote as an example: consider the use of a module to create a group of AWS instances as illustrated [here][link-4]. Yes, there is very little repetition in this code. The code is modular and re-usable. But is it _understandable_? Have I optimized for understandability, and (by extension) all the other needs listed in Sridharan's hierarchy of needs for code?

Consider this as well: have I _really_ violated the DRY principle if I were to explicitly spell out, with proper parameterization, the creation of each infrastructure object instead of using a `count` control structure or a module as a layer of abstraction? Is it not still true that there remains only "a single, unambiguous, authoritative representation" of each infrastructure object?

It seems to me that the latter approach---explicitly spelling out the creation of infrastructure objects in your infrastructure as code---may be a bit more verbose, but eminently more understandable and does not violate the DRY principle. It may not be as elegant, but as individuals creating infrastructure as code artifacts should be we optimizing for elegance, or optimizing for understandability?

Sridharan also talks about being explicit:

>...it is worth reiterating this again that implicit assumptions and dependencies are one of the worst offenders when it comes to contributing to the obscurity of code.

Again, it seems to me that---for infrastructure as code especially---being explicit about the creation of infrastructure objects not only contributes to greater understandability, but also helps eliminate implicit assumptions and dependencies. Instead of using a loop or control structure to manage the creation of multiple objects, spell out the creation of those objects explicitly. It may seem like a violation of the DRY principle to have three (nearly) identical snippets of code creating three (nearly) identical compute instances, but applying the DRY principle here means ensuring that each instance is authoritatively represented in the code only once, not that we are minimizing lines of code.

"Now wait," you say. "It's not my fault if someone can't read my Terraform code. They need to learn more about Terraform, and then they'll better understand how the code works."

Well, Sridharan talks about that as well in a discussion of properly identifying the target audience of your artifacts:

>In general, when identifying the target audience and deciding what narrative they need to be exposed to in order to allow for them to get up and running quickly, it becomes necessary to consider the audience’s background, level of domain expertise and experience.

Sridharan goes on to point out that in situations where both novices and veterans may be present in the target audience, the experience of the novice is key to determining the understandability of the code. So, if we are optimizing for understandability, can we afford to take a "hands off" approach to maintenance of the code by our successors? Can we be guaranteed that a successor tasked with maintaining our code will have the same level of knowledge and experience we have?

I'll stop here; there are more good points that Sridharan makes, but for now this post suffices to capture most of the thinking generated by the article when it comes to infrastructure as code. After I've had some time to continue to parse Sridharan's article, I may come back with some additional thoughts. In the meantime, feel free [to engage with me on Twitter][link-99] if you have some thoughts or perspectives you'd like to share on this matter.

[link-1]: https://medium.com/@copyconstruct/effective-mental-models-for-code-and-systems-7c55918f1b3e
[link-2]: https://packetpushers.net/podcast/full-stack-journey-027-understanding-infrastructure-as-code-and-terraform-with-curt-micol/
[link-3]: https://en.wikipedia.org/wiki/Don%27t_repeat_yourself
[link-4]: https://github.com/scottslowe/learning-tools/blob/master/terraform/aws/ic-module/main.tf
[link-99]: https://twitter.com/scott_lowe
