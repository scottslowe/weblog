---
author: slowe
categories: Information
comments: true
date: 2026-05-22T09:00:00-05:00
tags:
- Ansible
- AWS
- GitHub
- Hardware
- IaC
- Linux
- Networking
- Security
- Storage
- Terraform
- Virtualization
- VPN
- Web
title: Technology Short Take 196
url: 2026/05/22/technology-short-take-196
---

Welcome to Technology Short Take 196! Just in time for the US Memorial Day holiday, I am back with another list of articles related to various data center technologies like networking, security, operating systems, and applications. You will find articles on VPNs, Linux local privilege escalation (LPE) vulnerabilities, browser quirks and workarounds, the death of Terraform (again), and so much more. Enjoy your weekend reading!<!--more-->

## Networking

* Andrew Ward shares [an Ansible role to block abusive subnets using IPSet][link-9].
* From [this article on switching to WireGuard from Tailscale][link-25], I learned about [Headscale][link-26] (an open source implementation of the Tailscale control plane).

## Servers/Hardware

* Doug Dawson looks at [some innovations that continue to improve computing power][link-13]. Doug's article includes a few items I hadn't yet heard of.

## Security

* Bitwarden [confirmed a compromise of its CLI package on NPM][link-5].
* First, we had [CopyFail][link-6]. Now we have [Dirty Frag][link-7] and [Fragnesia][link-17].
* Daniel Stenberg talks about [how Mythos found a single vulnerability][link-19]---yes, just one---in `curl`.
* I don't really have words for this...[security incident][link-28]? That doesn't seem like the correct way to describe such a massive oversight, especially when you realize it happened to a cybersecurity agency. "Oops" doesn't even begin to describe it.

## Cloud Computing/Cloud Management

* Graham Gilbert insists [Terraform is dead][link-2].
* I wonder what sorts of things [the AWS MCP Server][link-14] will make possible?
* William Collins argues in favor of [AI through IaC rather than instead of it][link-15]. I enjoyed this article. Collins indicates that while authoring IaC using LLMs is working rather well, managing infrastructure using LLMs is another story entirely. If you're at all interested in the intersection of LLMs and IaC, I'd add this to your reading list.

## Operating Systems/Applications

* Eric Sloof looks at [using `llama.cpp` to run your own local, private AI stack on Apple Silicon][link-1].
* Mat Duggan examines his [descent from artist to mercenary][link-3] as a result of finding LLMs more useful than he expected or was prepared to accept.
* Chrome [silently installing a 4GB AI model][link-10] is definitely not cool. (I verified this myself on an Ubuntu 24.04 system running Chrome. It does not appear to happen with Chromium.)
* Teiva Harsanyi examines [how Linux 7.0 broke PostgreSQL][link-11].
* [Voice typing for the Linux desktop][link-12]...nice.
* For those of you that didn't already know, Rohini Gaonkar has a great explanation of [why AI lies to you][link-16].
* Jeff Butts [sings the praises of Homebrew on Linux][link-18].
* I must admit that I hadn't considered [linking LLMs with the four horsemen of the Apocalypse][link-21].
* The UNIX legacy [created so much more than just Linux][link-24].
* I really enjoyed Justin Garrison's [assessment of AI-generated content and its impact on people][link-27].

## Programming/Development

* [Mitchell Hashimoto and Ghostty are leaving GitHub][link-4].
* Den Odell [examines the quirks and workarounds that browsers implement][link-20] because of the Chrome monoculture.
* James Shore explains why [you need AI that _reduces_ maintenance costs][link-22].
* Rory McCune discusses [the rise of what he calls "personal software."][link-23]

## Career/Soft Skills

* Ky Decker describes their experience with [AI burnout][link-8].

That's a wrap! I hope you found something interesting or helpful to you. If you have links you would like me to share in a future Tech Short Take, feel free to send them my way. My email address is not hard to find or figure out, or you can reach me on [Mastodon][link-97], [Bluesky][link-98], and [Twitter/X][link-99]. I also lurk in a few different Slack communities, and you are welcome to drop me a DM there. Thanks for reading!

[link-1]: https://www.ntpro.nl/blog/archives/3871-Running-a-Local-Private-AI-Stack-on-Apple-Silicon-with-llama.cpp-and-Open-WebUI.html
[link-2]: https://grahamgilbert.com/blog/2026/04/20/terraform-is-dead/
[link-3]: https://matduggan.com/i-sold-out-for-200-a-month-and-all-i-got-was-this-perfectly-generated-terraform/
[link-4]: https://mitchellh.com/writing/ghostty-leaving-github
[link-5]: https://linuxiac.com/bitwarden-confirms-short-lived-npm-compromise-affecting-cli-package/
[link-6]: https://www.cyberkendra.com/2026/04/a-732-byte-python-script-can-get-root.html
[link-7]: https://www.bleepingcomputer.com/news/security/new-linux-dirty-frag-zero-day-with-poc-exploit-gives-root-privileges/
[link-8]: https://ky.fyi/posts/ai-burnout
[link-9]: https://blog.rubenwardy.com/2026/04/25/ansible-block-ipset/
[link-10]: https://www.thatprivacyguy.com/blog/chrome-silent-nano-install/
[link-11]: https://read.thecoder.cafe/p/linux-broke-postgresql
[link-12]: https://github.com/zugaldia/speedofsound
[link-13]: https://potsandpansbyccg.com/2026/05/08/who-needs-moores-law/
[link-14]: https://aws.amazon.com/blogs/aws/the-aws-mcp-server-is-now-generally-available/
[link-15]: https://wcollins.io/posts/2026/ai-through-iac-not-instead-of-it/
[link-16]: https://dev.to/aws/why-does-ai-lie-hallucinations-explained-simply-1c7g
[link-17]: https://github.com/v12-security/pocs/tree/main/fragnesia
[link-18]: https://www.xda-developers.com/homebrew-good-linux-mac/
[link-19]: https://daniel.haxx.se/blog/2026/05/11/mythos-finds-a-curl-vulnerability/
[link-20]: https://denodell.com/blog/browsers-treat-big-sites-differently
[link-21]: https://anarc.at/blog/2026-05-16-four-horsemen/
[link-22]: https://www.jamesshore.com/v2/blog/2026/you-need-ai-that-reduces-your-maintenance-costs
[link-23]: https://raesene.github.io/blog/2026/05/10/personal-software-and-baremetalvmm/
[link-24]: https://club.unix.rocks/commentary/under-linux/
[link-25]: https://the.unknown-universe.co.uk/home-lab/wireguard-vpn-two-vps/
[link-26]: https://headscale.net/stable/
[link-27]: https://justingarrison.com/blog/2026-05-19-your-slop-my-sludge/
[link-28]: https://krebsonsecurity.com/2026/05/cisa-admin-leaked-aws-govcloud-keys-on-github/
[link-97]: https://fosstodon.org/@scottslowe
[link-98]: https://bsky.app/profile/scottslowe.bsky.social
[link-99]: https://twitter.com/scott_lowe
