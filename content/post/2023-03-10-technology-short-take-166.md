---
author: slowe
categories: Information
comments: true
date: 2023-03-10T08:30:00-06:00
tags:
- Automation
- AWS
- Cilium
- Encryption
- ESXi
- Fedora
- Git
- Hardware
- Kubernetes
- Linux
- macOS
- Networking
- OPA
- Security
- Storage
- Terraform
- Virtualization
- VMware
title: 'Technology Short Take 166'
url: /2023/03/10/technology-short-take-166/
---

Welcome to Technology Short Take #166! I've been collecting links for the last few weeks, and now it's time to share them with all of you. There are some familiar names in the links below, but also some newcomers---and I'm really excited to see that! I'm constantly on the lookout for new sources (if you have a site you think I should check out, hit me up---my contact info is at the bottom of this post!). But enough of that, let's get on with the content. Enjoy!<!--more-->

## Networking

* Kevin Jin's [post on the APNIC blog about network automation tools][link-4] is a great read. He discusses Netmiko, NAPALM, and Nornir in some detail, and provides some guidance around which network automation tool may be right for you.
* Rob Novak shares his experience in [replacing Meraki with TP-Link Omada][link-18].
* Anton Kuliashov writes about [why Palark uses Cilium for Kubernetes networking][link-23].
* Nick Buraglio discusses [IPv6 Unique Local Addressing (ULA)][link-27].
* Given how frequently I include content from Ivan, you can probably tell that I'm a big fan. This time I'm including _two_ of his articles. In the first, he discusses [the relationships between Layer 2 (VLAN) and Layer 3 (subnet) segments][link-28]. In the second, Ivan unpacks news of recent vulnerabilities [hiding malicious packets behind the LLC SNAP header][link-29]. Both are great reads. But then when is one of Ivan's posts not a great read?

## Servers/Hardware

* Kevin Houston provides [some instructions][link-16] on backing up the Dell PowerEdge MX7000 settings and configurations.
* Frank Denneman [digs into Sapphire Rapids memory configurations][link-17] (Sapphire Rapids is Intel's 4th generation Scalable Processor Architecture).
* William Lam shares [news about higher-capacity SODIMMs available][link-19] for small or ultra small form factor systems.

## Security

* Jeff Warren [discusses][link-6] a potential way for malicious players to bypass multi-factor authentication, aka the "Pass the Cookie" attack.
* Aditya Patel [takes a closer look][link-7] at AWS' recent announcement to enable server-side encryption (SSE) on S3 _by default_, and whether this new default setting offers any real improvement in security posture. I won't spoil you by sharing his conclusion; go read the article (which is really well-written, in my opinion) to find out for yourself.
* Alberto Pellitteri with Sysdig [discusses SCARLETEEL][link-13], an operation conducted by an attacker that leveraged many of the tools found in modern cloud environments: Kubernetes, Terraform, and AWS. I highly recommend reviewing the article and considering what takeaways apply to your environment, if any.
* Martin Smolár of ESET provides [the first public analysis][link-14] of a UEFI bootkit that is capable of bypassing UEFI Secure Boot, a bootkit known as BlackLotus.

## Cloud Computing/Cloud Management

* [This article][link-1] on using Open Policy Agent (OPA) as a custom Lambda authorizer for the AWS API Gateway was informative and helpful. It did underscore something for me, though: I need to improve my coding skills.
* Do you need Argo CD? [This article][link-25] by Kirill Shirinkin provides, in the author's words, "some guidelines that will help you to assess if Argo CD makes sense for your setup".

## Operating Systems/Applications

* Stewart X Addison [shares][link-2] some macOS "first experiences" from a Linux user.
* Simon Willison [shares a "Today I Learned" entry about `sips`][link-3], a command line-based image processing utility for macOS.
* Kaleidoscope is my "go to" diff tool on macOS, and so I found this article on [using Kaleidoscope to compare binary files][link-5] interesting. In the future, I think I'll use this article to help folks understand why I prefer the terminal, too; compare the GUI solution via Shortcuts to the terminal solution at the end!
* Recently the runwasi project joined containerd, enabling containerd to support WASM (WebAssembly) containers. [This article][link-9] provides more details. Given containerd's prevalence---the article points out it's also behind Docker---is this the nudge that WASM containers need to go mainstream?
* [Via Nick Schmidt][link-10], I learned about D2, a language for "diagrams as code" using a DSL. I'll be giving this a spin soon, but in the meantime if this sounds interesting to you check out [the D2 GitHub repository][link-11].
* Yorick Peterse shares [his experience with Fedora Silverblue][link-12].
* Diego Crespo talks about [PowerShell on Linux and his experience with it][link-15].
* Here's an article with some [useful tips on improving Git performance][link-20] (note that most of this applies to large repositories).
* `ffmpeg` is such a useful tool (prompted by [this article][link-26]).

## Storage

* This is a good (albeit slightly dated) [overview of EBS volume types][link-8].
* Ever wondered how much data you can store in a single S3 bucket? [Wonder no longer][link-24].

## Virtualization

* Frank Denneman discusses [simulating NUMA nodes for nested ESXi virtual appliances][link-21].
* If you're into the VMware homelab thing (and I know quite a few folks are), then William's article on [interesting homelab kits for 2023][link-22] might be right up your alley.

I don't have any career/soft skills links for you this time, so that's all for now! I hope that I've included something that you'll find useful. As always, I invite your feedback on this post or any post on my site; feel free to reach out to [me on Twitter][link-99] or find [me on Mastodon][link-98]. I'm also present in a number of Slack communities, and you're welcome to contact me directly there as well. Thank you for reading!

[link-1]: https://aws.amazon.com/blogs/opensource/creating-a-custom-lambda-authorizer-using-open-policy-agent/
[link-2]: https://sxatech.blogspot.com/2022/03/macos.html
[link-3]: https://til.simonwillison.net/macos/sips
[link-4]: https://blog.apnic.net/2023/02/13/automation-tools-paramiko-netmiko-napalm-ansible-nornir-or/
[link-5]: https://blog.kaleidoscope.app/2023/02/06/compare-binary-data-using-kaleidoscope/
[link-6]: https://blog.netwrix.com/2022/11/29/bypassing-mfa-with-pass-the-cookie-attack/
[link-7]: https://www.secwale.com/p/encryption
[link-8]: https://cloudonaut.io/cheap-durable-fast-how-to-choose-an-ebs-volume-type/
[link-9]: https://www.infoq.com/news/2023/02/containerd-wasi/
[link-10]: https://blog.engyak.co/2023/03/d2-diagramming-intro
[link-11]: https://github.com/terrastruct/d2
[link-12]: https://yorickpeterse.com/articles/switching-to-fedora-silverblue/
[link-13]: https://sysdig.com/blog/cloud-breach-terraform-data-theft/
[link-14]: https://www.welivesecurity.com/2023/03/01/blacklotus-uefi-bootkit-myth-confirmed/
[link-15]: https://www.deusinmachina.net/p/powershell
[link-16]: http://www.bladesmadesimple.com/2023/02/backing-up-the-dell-poweredge-mx7000-settings-and-configurations/
[link-17]: https://frankdenneman.nl/2023/02/28/sapphire-rapids-memory-configuration/
[link-18]: https://rsts11.com/2023/02/19/replacing-meraki-with-tp-link-omada-for-the-new-year/
[link-19]: https://williamlam.com/2023/02/heads-up-24gb-48gb-ddr5-sodimm-memory-now-available.html
[link-20]: https://www.git-tower.com/blog/git-performance/
[link-21]: https://frankdenneman.nl/2023/03/02/simulating-numa-nodes-for-nested-esxi-virtual-appliances/
[link-22]: https://williamlam.com/2023/03/interesting-vmware-homelab-kits-for-2023.html
[link-23]: https://blog.palark.com/why-cilium-for-kubernetes-networking/
[link-24]: https://blog.kochie.io/articles/08-s3-file-limit
[link-25]: https://mkdev.me/posts/why-and-when-do-you-need-argo-cd
[link-26]: https://nickjanetakis.com/blog/create-video-clips-with-ffmpeg-in-seconds
[link-27]: https://forwardingplane.net/2022/11/04/the-mess-of-ipv6-unique-local-addressing/
[link-28]: https://blog.ipspace.net/2023/01/l2-l3-segments.html
[link-29]: https://blog.ipspace.net/2023/01/hiding-packets-behind-llc-headers.html
[link-98]: https://fosstodon.org/@scottslowe
[link-99]: https://twitter.com/scott_lowe
