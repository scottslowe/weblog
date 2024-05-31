---
author: slowe
categories: Information
comments: true
date: 2024-05-31T06:30:00-06:00
tags:
- AWS
- Hardware
- Kubernetes
- Linux
- Networking
- Pulumi
- Security
- Storage
- Virtualization
- VPN
title: 'Technology Short Take 178'
url: /2024/05/31/technology-short-take-178/
---

Welcome to Technology Short Take #178! This one is notably shorter than many of the Technology Short Takes I publish; I'm still trying to fine-tune my collection of RSS feeds (such a useful technology that seems to have fallen out of favor), removing inactive feeds and looking for new feeds to replace them. Regardless, I have managed to collect a few links for your reading pleasure this weekend. Enjoy!<!--more-->

## Networking

* Marc Brooker [discusses TCP_NODELAY in modern systems][link-6].
* [Because...why not][link-18]?

## Security

* Matt Moore, CTO of Chainguard, [goes into some detail][link-1] on how Chainguard intends to honor the principles behind the CISA's Secure by Design pledge.
* Ars Technica [examines TunnelVision][link-7], a vulnerability that has existed since 2002 and has the potential to render VPN apps useless. From my reading of the article, the greatest concern lies with untrusted networks where an attacker could manipulate things in their favor. Join that Wi-Fi network at the coffee shop at your own risk!
* Here's a slightly older post (March 2023) on [using AppArmor to restrict app permissions][link-17], with a particular focus on containers (including Kubernetes). It's a bit basic, but it does (in my opinion) provide some useful information.
* Nick Frichette shares some research around [using non-production AWS API endpoints as a potential attack surface][link-19].

## Cloud Computing/Cloud Management

* Yan Cui lays out [how to manage Route 53 hosted zones in multi-account environments][link-9]. Yan notes it's a problem for IaC products to deal with multiple accounts. For Pulumi, at least, this isn't typically an issue---although I haven't tried the specific instance that Yan mentions in the article (an ACM certificate request originating in a different account than where the domain is hosted).
* [This trick][link-12] for "de-Googling Google" has been making the rounds on social medial for the last few days. I haven't personally tried it yet---have you?
* If you're interested in blocking the bots that various companies use to scrape your site for LLM training data, there's some good information [here][link-14].
* Muhammad Bhatti shares some [code and information for "bootstrapping" Pulumi][link-15] (that is, using Pulumi to create the necessary AWS infrastructure for a self-managed backend).
* Jacob Gillespie of Depot shares some neat tricks for [making EC2 boot time 8x faster][link-16].

## Operating Systems/Applications

* I'm in agreement with Simon Willison and others on [the use of "slop"][link-2] to describe the artificially generated content that is increasingly polluting search results.
* Jaiden Riordan takes readers on a journey of [running Ultramarine Linux on quite a wide spectrum of computing hardware][link-10].
* Duane Johnson offers [a comprehensive look at key remapping options in Linux][link-13] (as of late 2021).

## Storage

* Major Hayden [discusses the "amazon-ec2-utils" package][link-8] that's on its way to the stable repos in Fedora, and how it helps align the device names used by the AWS API and the Linux instance running on EC2.

## Career/Soft Skills

* [Betty Junod has had enough of imposter syndrome][link-5]. I really appreciated this article; my favorite line is "I am NOT an imposter, I am new here." So true.
* [This article][link-4] on overcoming shyness at conferences dates all the way back to 2013, but is still applicable today.
* Kat Traxler shares her [process for preparing for talks][link-3].
* Gregor Hohpe discusses [the economics of writing technical books][link-11].

That's all for now! I sincerely hope you found something useful among these links. As always, I welcome any and all feedback; find me online and let me know what you think of this post or the site in general. I'm [on Twitter][link-99], in [the Fediverse][link-30], it's not hard to locate me in any of the various Slack communities I frequent, and I even take the time to respond to legitimate e-mail messages from readers. Don't be shy, reach out and say hi!

[link-1]: https://www.chainguard.dev/unchained/signing-cisas-secure-by-design-pledge
[link-2]: https://simonwillison.net/2024/May/8/slop/
[link-3]: https://kattraxler.cloud/musings/2024/05/08/how-i-prep-for-talks.html
[link-4]: http://blog.pamelafox.org/2013/10/shyness-hacks-for-conferences.html
[link-5]: https://www.bettyjunod.com/blog/dont-let-them-call-you-an-imposter
[link-6]: https://brooker.co.za/blog/2024/05/09/nagle.html
[link-7]: https://arstechnica.com/security/2024/05/novel-attack-against-virtually-all-vpn-apps-neuters-their-entire-purpose/
[link-8]: https://major.io/p/amazon-ec2-utils-fedora/
[link-9]: https://theburningmonk.com/2021/05/how-to-manage-route53-hosted-zones-in-a-multi-account-environment/
[link-10]: https://blog.fyralabs.com/um-where-it-shouldnt-be/
[link-11]: https://architectelevator.com/strategy/book-author-economics/
[link-12]: https://tedium.co/2024/05/17/google-web-search-make-default/
[link-13]: https://medium.com/@canadaduane/key-remapping-in-linux-2021-edition-47320999d2aa
[link-14]: https://neil-clarke.com/block-the-bots-that-feed-ai-models-by-scraping-your-website/
[link-15]: https://justedagain.com/posts/2022/pulumi-backend-bootstrap/
[link-16]: https://depot.dev/blog/faster-ec2-boot-time
[link-17]: https://www.sobyte.net/post/2023-03/apparmor/
[link-18]: https://blog.jonasbengtson.se/cisco-7609-beer-tap
[link-19]: https://securitylabs.datadoghq.com/articles/non-production-endpoints-as-an-attack-surface-in-aws/
[link-30]: https://fosstodon.org/@scottslowe
[link-99]: https://twitter.com/scott_lowe
