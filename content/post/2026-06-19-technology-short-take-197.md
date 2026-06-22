---
author: slowe
categories: Information
comments: true
date: 2026-06-19T09:00:00-05:00
tags:
- AWS
- BSD
- Encryption
- Hardware
- IaC
- Kubernetes
- Linux
- Networking
- Pulumi
- Security
- Storage
- Virtualization
title: Technology Short Take 197
url: 2026/06/19/technology-short-take-197
---

Welcome to Technology Short Take 197! I've been traveling for business for the last week, so this Technology Short Take has a tad fewer links than I typically include. Even so, I still have links on radical new network designs, the impacts of AI on code security, things beginners get wrong about AWS IAM, and more! Let's get into the content.<!--more-->

## Networking

* This story about [a radical network redesign at AWS][link-7]---along with [this accompanying arXiv paper][link-8]---was intriguing to me. I'm not a networking expert, but designing networks with quasi-random connections between endpoints _definitely_ flies in the face of the traditional wisdom. I do wonder, though, if this is the sort of technology that only makes sense for the hyperscalers, and not for the rest of us.
* Doug Dawson provides a quick update on current encryption standards and what he terms [the "pending encryption crisis" presented by quantum computing][link-15].
* It would appear that [AI is affecting even Cisco's well-known industry certifications][link-18], as outlined by Daniel Dib.

## Security

* CodeRabbit shared the results of an analysis of PRs to open source projects showing that [AI-written code produces ~1.7x more issues][link-10].
* Along the same lines, Jens Wessling of Veracode indicates that [45% of all AI-generated code samples contained OWASP Top 10 security vulnerabilities][link-11].
* Anyone who didn't expect [seemingly-dangerous AI agent security exploits][link-19] to appear hasn't been paying attention. [Enterprise AI tools don't appear exempt, either][link-20].
* The complex interdependencies of open source software projects once again means that [a single critical vulnerability threatens a much larger number of projects and tools][link-24].

## Cloud Computing/Cloud Management

* This is an older post, but the author recently updated it---check out Marcus Noble's [recommended resources for Kubernetes newbies][link-1].
* I am not yet sure about `pulumi do` (direct operations for any cloud resource; see [the Pulumi blog post][link-3] about it). On one hand, I can see the utility in situations where you "just need a quick" whatever. On the other hand, isn't this kind of antithetical to the core premise of infrastructure as code?
* The scale of [the AI bubble][link-6] is staggering.
* Jonathan Vogel explains [what every AWS beginnner gets wrong about IAM][link-13]. I thought his metaphor comparing users and roles to employee badges and visitor passes was a good one.
* Rose Security has a comprehensive blog post on [building an AWS image factory][link-14] (to create custom AMIs).

## Operating Systems/Applications

* [Omarchy is not a distribution, just a collection of dotfiles.][link-4]
* George Whittaker [examines the Flatpak sandbox and some of its real-world security issues][link-5].
* OpenBSD 7.9, the 60th OpenBSD release, was [recently made available][link-9].

## Programming/Development

* Mark Russinovich and Scott Hanselman authored [this ACM paper discussing the effects of AI on early-in-career (EiC) developers][link-12]. I found some powerful insights here---this is well worth a read.

## Storage

* Stephen Akinyemi of Microsandbox describes [how they made their filesystem 47x faster by deleting it][link-2].

## Virtualization

* William Lam has published [a spate of articles on VCF 9.1][link-16] over the past couple of weeks. If VCF is your jam, then you're bound to find some useful information in these posts.
* And speaking of VCF 9.1, Eric Sloof [melds the LLM work he's been doing recently with the VCF documentation][link-17]. I believe this is a good use of local models.

## Career/Soft Skills

* I included a link to [this cautionary tale of how KPIs kill efficiency][link-21] in the hope of helping the manager-types among my readers prevent this from happening to their organizations/teams.
* I found this quote about [the use of AI tools in the workplace][link-22] pertinent: "Use the tool where you can verify precisely what it produces."
* "Well, I’m willing to bet that you, my fellow technologist, have benefitted from the distributed technology community on countless occasions." Yes, I absolutely have! And because of that, I wouldn't want to use tools that threaten this community, and that's why I appreciated this discussion that [the community is the achievement and the achievement is the community][link-23].

That is all I have for you this time around---but I'll be back in a couple weeks with another Technology Short Take and another set of links to data center technologies and related areas. In the meantime, if you come across something interesting you feel I should include in a future Tech Short Take, feel free to send it my way! You can reach me via [Mastodon][link-97], [Bluesky][link-98], [Twitter/X][link-99], several different Slack communities, or even email (my address isn't hard to find). Thanks for reading!

[link-1]: https://marcusnoble.co.uk/2024-06-24-my-recommended-kubernetes-resources-for-newbies/
[link-2]: https://microsandbox.dev/blog/oci-filesystem-47x-faster
[link-3]: https://www.pulumi.com/blog/pulumi-do-direct-resource-operations/
[link-4]: https://abyss.fish/your_dotfiles_are_not_a_distro
[link-5]: https://www.linuxjournal.com/content/when-flatpaks-sandbox-cracks-real-life-security-issues-beyond-ideal
[link-6]: https://nooneshappy.com/article/the-ai-bubble/
[link-7]: https://www.aboutamazon.com/stories/aws-random-graph-theory-data-center-network-design
[link-8]: https://arxiv.org/pdf/2604.15261
[link-9]: https://www.openbsd.org/79.html
[link-10]: https://www.coderabbit.ai/blog/state-of-ai-vs-human-code-generation-report
[link-11]: https://www.veracode.com/blog/genai-code-security-report/
[link-12]: https://dl.acm.org/doi/10.1145/3779312
[link-13]: https://dev.to/aws/access-denied-what-every-aws-beginner-gets-wrong-about-iam-9k1
[link-14]: https://rosesecurity.dev/2026/05/20/building-an-image-factory.html
[link-15]: https://potsandpansbyccg.com/2026/06/04/the-pending-encryption-crisis/
[link-16]: https://williamlam.com/category/vmware-cloud-foundation
[link-17]: https://www.ntpro.nl/blog/archives/3874-Chatting-with-9,000-Pages-of-VMware-VCF-9.1-Documentation-Locally,-Free,-and-Private.html
[link-18]: https://lostintransit.se/2026/05/21/cisco-launches-major-updates-to-certifications/
[link-19]: https://www.varonis.com/blog/reprompt
[link-20]: https://www.varonis.com/blog/searchleak
[link-21]: https://vincent.bernat.ch/en/blog/2026-kpi-goodhart
[link-22]: https://nooneshappy.com/article/appearing-productive-in-the-workplace/
[link-23]: https://linguacelta.com/blog/2026/05/LLMs.html
[link-24]: https://arstechnica.com/information-technology/2026/05/millions-of-ai-agents-imperiled-by-critical-vulnerability-in-open-source-package/
[link-97]: https://fosstodon.org/@scottslowe
[link-98]: https://bsky.app/profile/scottslowe.bsky.social
[link-99]: https://twitter.com/scott_lowe
