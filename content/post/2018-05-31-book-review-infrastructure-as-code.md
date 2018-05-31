---
author: slowe
categories: Review
comments: true
date: 2018-05-31T12:00:00Z
tags:
- Cloud
- Development
- Storage
- Networking
- Security
- Automation
title: "Book Review: Infrastructure as Code"
url: /2018/05/31/book-review-infrastructure-as-code/
---

As part of [my 2018 projects][xref-1], I committed to reading and reviewing more technical books this year. As part of that effort, I recently finished reading _Infrastructure as Code_, authored by Kief Morris and published in September 2015 by O'Reilly (more details [here][link-1]). Infrastructure as code is very relevant to my current job function and is an area of great personal interest, and I'd been half-heartedly working my way through the book for some time. Now that I've completed it, here are my thoughts.<!--more-->

Overall, Morris does a great job of crisply defining infrastructure as code (a somewhat vague and amorphous term at times) and outlining the key principles that are involved. Morris also does a really good job of staying high-level as he works through the various aspects of infrastructure as code and discusses some of the considerations, patterns (and anti-patterns), and recommended practices in each aspect.

The book's high-level focus is, however, both its greatest strength as well as its greatest weakness. Because infrastructure as code can be implemented in a variety of ways with a variety of tools, the book must necessarily be high-level and somewhat abstract. As I mentioned, Morris does a really good job with this. In remaining high-level, though, the book loses some of the details and tool-specific recommendations that would be helpful in getting readers started implementing infrastructure as code in their own environments. Although Morris makes brief references to specific tools here and there, there simply isn't room for him to discuss any one tool in any depth.

The result?

* For readers who are already familiar with tools like [Terraform][link-3] or [Ansible][link-4] (or similar), Morris' review of key concepts, considerations, patterns/anti-patterns, and practices enables them to better understand the big picture and where their tool-specific skills fit into that big picture. In some aspects, Morris is complementing these readers' "tactical skills" with some "strategic skills."
* For readers who are _not_ familiar with tools commonly used in infrastructure as code, the concepts are so high-level and abstract that it can be difficult at times to translate them into specific tasks that can be accomplished with a particular tool. The question of "OK, how would I do that in CloudFormation?" (or Terraform, or Ansible, or any other such tool) isn't a question Morris can answer because the book isn't written at that level.

This is not to say that the book isn't helpful; quite the contrary! Morris' identification of key patterns to adopt and key anti-patterns to avoid helps readers understand what (and what not) to do as they progress along their journey with infrastructure as code. This in and of itself is very useful for readers of all skill levels, in my opinion. Additionally, Morris calls out numerous non-technological considerations along the way; this quote (from Chapter 9) was one of my favorites:

>When you find yourself considering organizational processes to coordinate and schedule changes, stop!

Here, Morris is warning against allowing infrastructure definitions to become unwieldy and monolithic, which would increase the likelihood that changes would cause something to break and thus lead people to avoid changes that are necessary. Instead of creating organizational processes to protect against the automation fear spiral (another great concept that Morris introduces early in the book), he recommends refactoring your infrastructure definitions to make them more modular, more flexible, and more resilient to change. This is just one example of content that is enormously useful to all sorts of readers, and this sort of wisdom/experience is sprinkled liberally throughout the book.

Overall, I _highly_ recommend this book. Be aware, though, that this book won't give you the tool-specific knowledge you'll need to implement the concepts that Morris introduces and explains, so you'll want to pair this book with a more hands-on, tool-specific tome. For example, users wanting to implement infrastructure as code using something like Terraform should combine this book with something like _Terraform: Up and Running_, also by O'Reilly (see [here][link-2] for details).

_Disclaimer: I am an O'Reilly author, but I did not receive any compensation from O'Reilly, any other publisher or distributor, or the authors of the books mentioned in this review. These thoughts are mine alone._

[link-1]: http://shop.oreilly.com/product/0636920039297.do
[link-2]: http://shop.oreilly.com/product/0636920061939.do
[link-3]: https://www.terraform.io/
[link-4]: https://www.ansible.com/
[xref-1]: {{< relref "2018-03-05-looking-ahead-2018-projects.md" >}}
