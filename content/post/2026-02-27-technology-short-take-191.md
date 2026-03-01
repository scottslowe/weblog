---
author: slowe
categories: Information
comments: true
date: 2026-02-27T09:00:00-05:00
tags:
- Ansible
- Automation
- Cisco
- GRE
- Hardware
- IaC
- KVM
- Linux
- macOS
- Networking
- OSPF
- Productivity
- Pulumi
- Security
- Storage
- Terraform
- Virtualization
- VXLAN
- Wireless
title: Technology Short Take 191
url: 2026/02/27/technology-short-take-191
---

Welcome to Technology Short Take #191! This is my semi-regular collection of links related to technology disciplines, including networking, security, cloud computing, storage, and programming/development. I hope that I've managed to curate an interesting and useful set of links for readers. Enjoy!<!--more-->

## Networking

* I learned a new acronym from Ivan Pepelnjak in his article on ["not so passive" OPSFv2 interfaces in Cisco IOS/XR][link-16]: QDS (Quick and Dirty Solution). I am totally using that from now on.
* Via Ivan (hat tip to my colleague Russ for pointing me in this direction), I also learned that [all is not well in network automation land with Ansible][link-22]. Although some parts of it are apparently now resolved, this does [raise questions about the viability of Ansible for network automation][link-23].
* Doug Dawson [talks about the new Amazon Eero Signal][link-24], a cellular backup to a primary broadband connection for users using their Eero mesh systems. I agree with Doug---the pricing seems high to me. I _generally_ like the Eero devices, although there have been times when I really could use a bit more control over the settings.

## Security

* The APNIC blog features a guest article discussing [the use of tunneling protocols to infiltrate networks][link-8]. The article specifically discusses Generic Routing Encapsulation (GRE) and Virtual Extensible LAN (VXLAN). It's worth a read if your network uses either of these technologies.

## Cloud Computing/Cloud Management

* Somewhere along the way I missed [the CDK deprecation announcement][link-3]. If I am not mistaken, this leaves Pulumi as the only cloud-independent IaC tool that lets you use a general purpose programming language.
* Managing dependencies between containers can be difficult at times; [this blog post by Nicolas Fränkel][link-4] introduced me to `wait4x`, a tool one can use to help simplify these types of situations.

## Operating Systems/Applications

* I appreciated reading [Mitchell Hashimoto's AI adoption journey][link-2]. I feel it does provide a more realistic approach to how someone in a role like his might leverage an LLM or coding agent.
* Michael Christofides taught me about bloat in this article on [read efficiency issues in Postgres queries][link-5].
* Ricardo Katz discusses [a few somewhat-recent security vulnerabilities in kgateway][link-6], an open source Gateway API implementation that leverages Envoy Proxy.
* I am squarely in agreement with Martin Fowler on [the topic of agentic email][link-15]---this seems like a dangerous combination, so tread carefully. (I am squarely in disagreement with Martin's [characterization of servant leadership as gaslighting][link-18], but that's another topic for another day.)
* You know AI has gone too far when you find it has ["continvoucly morged" your content][link-17].
* Earlier this week I stumbled across [`hazelnut`, a terminal-based file organizer for Linux systems][link-19]. The macOS app Hazel inspired `hazelnut`. Neat!

## Programming/Development

* Tuan Nguyen's article on [parsing a YAML file in Golang][link-7] was helpful for me recently. I had struggled with how to do this without a struct, and the article provides a couple of examples of using a map instead of a struct for this purpose.
* Simon Willison is [starting to write about agentic engineering patterns][link-25]---that is, patterns for software engineering when working with agents.

## Virtualization

* Lightweight Linux VMs (sometimes called "micro-VMs") are gaining in popularity. Deno recently [introduced Deno Sandbox][link-1], built on lightweight Linux VMs, to provide code security for untrusted code.
* William Lam provides some information on [using cross-vCenter vMotion to help with migrating away from vSphere 7][link-9] (which is now reached end of support).
* Who out there is messing around with [Incus][link-20]? I am considering trying it out.
* I noticed this past week that you can now [use nested virtualization to run hypervisors on "regular" EC2 instances][link-21].

## Career/Soft Skills

* Tom Hollingsworth writes about [managing your attention][link-10], which is something I'm increasingly seeing. I spoke about this some time ago when I advised folks to turn off new email notifications and switch to checking your email less frequently so you could focus on getting work done.
* I also saw this post by Scott H Young about [managing your energy instead of your time][link-11], which---to me, at least---relates to Tom's post in the previous bullet. In [the follow-up post explaining what energy is][link-12], Young (or one of the linked articles/studies) refers to energy as a "mental resource." It seems to me, as an uneducated layman, that managing your energy levels would also entail managing the cognitive zap you get from constant context switches due to notifications. I could be wrong (it wouldn't be the first time, nor will it be the last).
* One final thought in this thread: Scott Young's article on [how to make hard work feel less hard][link-13] talks about making tasks less effortful. This sounds a lot like "reducing the friction" for common tasks, which I have discussed in the past (here is [one example][xref-1]). Friction is something that [Murat Demirbas has also recently discussed][link-14], although his example is more organizational than personal. Either way, the concept of friction in our work is definitely a valid concept, and something we should take into account as we work toward managing both our attention and our energy.

That's all for now. I will return in a couple of weeks with the next installation, and a fresh set of links from around the internet. In the meantime, I invite you to drop me a line (my email is not hard to find) or hit me up on social media ([X/Twitter][link-99], [Mastodon][link-98], [Bluesky][link-97], or [LinkedIn][link-96]) and let me know if you've found something useful. I love to hear from readers!

[link-1]: https://deno.com/blog/introducing-deno-sandbox
[link-2]: https://mitchellh.com/writing/my-ai-adoption-journey
[link-3]: https://www.pulumi.com/blog/cdktf-is-deprecated-whats-next-for-your-team/
[link-4]: https://blog.frankel.ch/subtle-art-waiting/
[link-5]: https://www.pgmustard.com/blog/read-efficiency-issues-in-postgres-queries
[link-6]: https://dev.to/rkatz/the-kgateway-vulnerabilities-explained-and-why-i-disagree-on-its-score-339e
[link-7]: https://www.golinuxcloud.com/golang-parse-yaml-file/
[link-8]: https://blog.apnic.net/2026/01/16/from-spoofing-to-tunnelling-new-red-team-networking-techniques-for-initial-access-and-evasion/
[link-9]: https://williamlam.com/2026/02/cross-vcenter-vmotion-workloads-from-vsphere-7-0-to-vsphere-9-0.html
[link-10]: https://networkingnerd.net/2026/02/12/the-inattention-economy/
[link-11]: https://www.scotthyoung.com/blog/2026/01/14/manage-energy-not-time/
[link-12]: https://www.scotthyoung.com/blog/2026/01/21/what-exactly-is-energy/
[link-13]: https://www.scotthyoung.com/blog/2026/02/12/make-hard-work-feel-easy/
[link-14]: https://muratbuffalo.blogspot.com/2026/02/friction.html
[link-15]: https://martinfowler.com/bliki/AgenticEmail.html
[link-16]: https://blog.ipspace.net/2026/02/iosxr-ospfv2-not-so-passive/
[link-17]: https://nvie.com/posts/15-years-later/
[link-18]: https://martinfowler.com/bliki/HostLeadership.html
[link-19]: https://github.com/ricardodantas/hazelnut/
[link-20]: https://linuxcontainers.org/incus/
[link-21]: https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/amazon-ec2-nested-virtualization.html
[link-22]: https://blog.ipspace.net/2025/11/ansible-12-different/
[link-23]: https://blog.ipspace.net/2025/12/ansible-abandoned-network-automation/
[link-24]: https://potsandpansbyccg.com/2026/02/24/cellular-backup-for-broadband/
[link-25]: https://simonwillison.net/2026/Feb/23/agentic-engineering-patterns/
[link-96]: https://www.linkedin.com/in/scottslowe/
[link-97]: https://bsky.app/profile/scottslowe.bsky.social
[link-98]: https://fosstodon.org/@scottslowe
[link-99]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2013-05-31-reducing-the-friction-bbedit-to-marsedit.md" >}}
