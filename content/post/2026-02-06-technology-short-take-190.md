---
author: slowe
categories: Information
comments: true
date: 2026-02-06T08:00:00-05:00
tags:
- Apple
- AWS
- Azure
- Encryption
- Hardware
- macOS
- Markdown
- Messaging
- Networking
- Security
- Storage
- Terraform
- Virtualization
- VMware
- Writing
title: Technology Short Take 190
url: 2026/02/06/technology-short-take-190
---

Welcome to Technology Short Take #190! This is the first Tech Short Take of 2026, and it has been nearly three months (wow!) since the last one. I can't argue that I fell off the blogging bandwagon over the end of 2025 and early 2026. I won't get into all the reasons why (if you're interested then feel free to reach out and I'll fill you in). Enough about me---let's get to the technical content! Here's hoping you find something useful.<!--more-->

## Networking

* Here's a little something on the lighter side about [what causes network outages][link-19].
* As the lead-in to [this article on Starlink's performance during a solar superstorm][link-20] says, "It's not science fiction." Solar storms can have a real impact on Earth, and satellites in low-Earth orbit (LEO) are not exempt from that impact.

## Servers/Hardware

* William Lam [reviews the Mini PC and SFF (small form factor) hardware announcements from CES 2026][link-21].

## Security

* The scale of DDoS attacks continues to grow, as evidenced by [this report of a 15 Tbps attack on Microsoft Azure][link-1].
* Microsoft having the ability to [give away your Windows PC's data encryption key][link-18] is horrifying.

## Cloud Computing/Cloud Management

* While doing some reading on Terragrunt, I also came across [this open source tool for performing operations against multiple Git repositories][link-6]. I don't have a use case for it, but it is cool!
* While on the topic of Terragrunt: let me say that I appreciate the work that went into [their online docs][link-7]! From what I've read so far, they are well-written, clear, concise, and informative. Well done!
* And while still on the topic of Terragrunt: it was this article from Axel Mendoza on why they [use Terragrunt over Terraform/OpenTofu][link-8] that sent me down the Terragrunt rabbit hole.
* The most recent installation of [Ricardo Sueiras' AWS open source newsletter][link-11] pointed me to a couple of tools that look really handy: `s3sh` (available [from GitHub][link-12]) and `taws` (also available [from GitHub][link-13]).
* Nick Buraglio has [a great comparison of hosted email options][link-16].

## Operating Systems/Applications

* Howard Oakley's ["loss of confidence" in macOS][link-4] is one reason why I started using Linux more. I just kept finding things in macOS that didn't work like they should, which was made worse by the fact that Apple (the company) just doesn't seem to care.
* It is still kind of raw, but I found [a Markdown preview tool for Linux called Inlyne][link-9]. Pretty cool! I am looking forward to seeing how it develops.
* [This treatise from Mike Swanson on "backseat software"][link-10] is absolute gold. Go read it. Now. Seriously, this is good stuff. I was nodding so vigorously as I was reading it that I almost gave myself a headache!

## Storage

* Looking for some information on vSAN Encryption in VMware Cloud Foundation? I think Eric Sloof has [exactly the document you need][link-14]. (You may also find [this related article from Eric][link-15] useful as well.)

## Virtualization

* Eric Sloof writes about [the evolution of the VCDX][link-2]. As an early VCDX (number 39), I am of two minds regarding this change. On one hand, it makes sense; they need more than just architects to pursue this certification. On the other hand, they are (in some ways) "softening" the certification. It will be interesting to see if this helps the designation grow, or if it continues to fade into obscurity.

## Career/Soft Skills

* Murat Demirbas [talks a bit about momentum][link-3], and how maintaining momentum (the "the messy daily pushes" as he puts it) should be how you organize and structure your workflows. I can see his point, certainly, although I would warn folks against interpreting his "produce something every day" advice as meaning that you need to _publish_ something every day. That path leads to burn out. Produce something, sure, but allow for that something to be something that only you'll see.
* [This article about getting better at technical writing][link-5] contains some good guidelines.
* [This discussion of feeling uncomfortable and how it relates to growing personally][link-17] really resonated with me. No, not because of Werner's fear of public speaking; rather, it was about how it is necessary to move outside of your comfort zone in order to grow. I am getting ready to potentially embark on something that really pushes me outside my comfort zone, and it is scary. I also know, though, that it is the only way to grow.

That's all, folks! (Bonus points for you if you recognize that reference.) I am always up to hear from readers, so I invite you to reach out using various social media platforms ([X/Twitter][link-99], [Mastodon][link-30], [Bluesky][link-29], [LinkedIn][link-28]), Slack, or old-fashioned email. Thanks for reading!

[link-1]: https://www.bleepingcomputer.com/news/microsoft/microsoft-aisuru-botnet-used-500-000-ips-in-15-tbps-azure-ddos-attack/
[link-2]: https://www.ntpro.nl/blog/archives/3845-Introducing-the-VMware-Certified-Distinguished-Expert-VCDX-A-New-Era-for-Elite-Private-Cloud-Professionals.html
[link-3]: https://muratbuffalo.blogspot.com/2025/12/optimize-for-momentum.html
[link-4]: https://eclecticlight.co/2025/11/30/last-week-on-my-mac-losing-confidence/
[link-5]: https://www.seangoedecke.com/technical-communication/
[link-6]: https://github.com/gruntwork-io/git-xargs
[link-7]: https://terragrunt.gruntwork.io/docs/getting-started/quick-start/
[link-8]: https://www.axelmendoza.com/posts/terraform-vs-terragrunt/
[link-9]: https://github.com/Inlyne-Project/inlyne
[link-10]: https://blog.mikeswanson.com/backseat-software/
[link-11]: https://dev.to/aws/aws-open-source-newsletter-218-3e3m
[link-12]: https://github.com/dacort/s3sh
[link-13]: https://github.com/huseyinbabal/taws
[link-14]: https://www.ntpro.nl/blog/archives/3849-A-Technical-Overview-of-vSAN-Encryption-in-VMware-Cloud-Foundation.html
[link-15]: https://www.ntpro.nl/blog/archives/3831-vSAN-Encryption-Services-in-VMware-Cloud-Foundation-9.0.html
[link-16]: https://www.forwardingplane.net/post/2025-12-26-hosted-email-options-2026/
[link-17]: https://www.allthingsdistributed.com/2025/12/a-little-bit-uncomfortable.html
[link-18]: https://www.windowscentral.com/microsoft/windows-11/microsoft-bitlocker-encryption-keys-give-fbi-legal-order-privacy-nightmare
[link-19]: https://potsandpansbyccg.com/2026/01/19/rats-attack-fiber/
[link-20]: https://blog.apnic.net/2025/12/17/investigating-starlinks-performance-during-the-may-2024-solar-superstorm/
[link-21]: https://williamlam.com/2026/01/every-mini-pc-sff-hardware-announced-at-ces-2026.html
[link-28]: https://www.linkedin.com/in/scottslowe/
[link-29]: https://bsky.app/profile/scottslowe.bsky.social
[link-30]: https://fosstodon.org/@scottslowe
[link-99]: https://twitter.com/scott_lowe
