---
author: slowe
categories: Information
comments: true
date: 2024-01-05T08:30:00-06:00
tags:
- AWS
- Azure
- Docker
- ESXi
- Google
- Hardware
- Networking
- Pulumi
- Security
- Storage
- Terraform
- Virtualization
- VMware
title: 'Technology Short Take 172'
url: /2024/01/05/technology-short-take-172/
---

Welcome to Technology Short Take #172, the first Technology Short Take of 2024! This one is _really_ short, which I'm assuming reflects a lack of blogging activity over the 2023 holiday season. Nevertheless, I have managed to scrape together a few links to share with readers. As usual, I hope you find something useful. Enjoy!<!--more-->

## Networking

* Via [this blog post][link-11], I learned that Ivan Pepelnjak has [a GitHub repository of hands-on examples][link-12] for learning public cloud networking (including both AWS and Azure). Ivan's materials are always excellent, so if you're looking for resources to help with expanding your networking skills into the public cloud, this should be on the short list. (I plan to submit a PR soon to add Pulumi examples, which the repository is currently missing.)

## Cloud Computing/Cloud Management

* Jonathan Major shares his experience [using Pulumi with Google Cloud APIs][link-2]. I think there are some code snippets in Jonathan's article, but my instance of Firefox wouldn't render the code snippets (instead only showing empty black boxes).
* Nisar Ahmad [explores][link-3] whether Terraform or Pulumi is better for your use case.

## Operating Systems/Applications

* Nick Janetakis shares another tip for [determining how long a Docker container was running][link-8]. He uses `grep` against the output of `docker inspect`, but I'm wondering if there's a similar way of doing it using `jq`.
* [Here][link-9] is a list of eight Docker tips and tricks that you might find useful.

## Programming/Development

* [This post][link-1] explores the idea of writing games as a way of "reclaiming" the hobby of writing code (as a means for avoiding/preventing burnout).
* I enjoyed reading [this first post on the pragmatic engineer's philosophy][link-7]. Although this article references additional posts, it doesn't appear that any more posts on this topic have been published (yet).

## Virtualization

* William Lam shares a [new method for marking a hard drive as an SSD][link-10] in both ESXi 7.x and ESXi 8.x. As William mentions in the article, this can be useful when using nested ESXi or when the storage isn't properly recognized.

## Career/Soft Skills

* Yan Cui digs into the challenges around [breaking through the "senior engineer" career ceiling][link-4].
* While reading the article in the previous bullet, I was pointed to this article on [why someone wouldn't want to be a manager][link-5]. If, however, you've decided to become a manager anyway, then [this article][link-6] might be helpful to you.

That's all for this time around---short, sweet, and to the point! I always love to hear feedback from readers, so feel free to find me online and reach out. I'm [on Twitter][link-99], [on the Fediverse][link-30], and you can typically find me in a few different Slack communities (including the [Pulumi community Slack][link-29]). Thanks for reading!

[link-1]: https://threedots.tech/post/making-games-in-go/
[link-2]: https://www.linkedin.com/pulse/working-pulumi-google-cloud-apis-jonathan-major-wi3zf
[link-3]: https://dzone.com/articles/terraform-vs-pulumi-which-is-better-for-your-iac-r
[link-4]: https://theburningmonk.com/2019/11/how-to-break-the-senior-engineer-career-ceiling/
[link-5]: https://charity.wtf/2019/09/08/reasons-not-to-be-a-manager/
[link-6]: https://thenewstack.io/help-im-a-leader-now/
[link-7]: https://blog.liblab.com/pragmatic-engineers-philosophy-first-part/
[link-8]: https://nickjanetakis.com/blog/docker-tip-96-see-how-long-a-container-ran-for
[link-9]: https://www.docker.com/blog/8-top-docker-tips-tricks-for-2024/
[link-10]: https://williamlam.com/2024/01/quick-tip-new-method-to-mark-hdd-to-ssd-in-esxi-7-x-and-8-x-using-esxcli.html
[link-11]: https://blog.ipspace.net/2024/01/public-cloud-labs.html
[link-12]: https://github.com/ipspace/pubcloud
[link-29]: https://slack.pulumi.com/
[link-30]: https://fosstodon.org/@scottslowe
[link-99]: https://twitter.com/scott_lowe
