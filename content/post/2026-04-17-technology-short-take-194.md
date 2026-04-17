---
author: slowe
categories: Information
comments: true
date: 2026-04-17T08:00:00-05:00
tags:
- AWS
- CLI
- Development
- Docker
- Encryption
- Hardware
- IPv6
- JSON
- Linux
- Networking
- Security
- Storage
- Virtualization
title: Technology Short Take 194
url: 2026/04/17/technology-short-take-194
---

This is Technology Short Take #194, the latest in my semi-regular series of posts sharing data center technology-related links and articles from around the web. What have I gathered for readers this time? Key topics in Tech Short Take 194 include a new cryptography-based network stack, a look at why folks are revolting against AI, and an in-depth comparison of microVM technologies. On to the content!<!--more-->

## Networking

* [This is cool.][link-13]
* Longtime reader Todd Smith pointed me to [Reticulum][link-18], a new cryptography-based network stack. Todd also spent some time messing around with Reticulum and shared his findings in two blog posts: first, [an initial/introductory post][link-19], followed by a post sharing [his experience with some Reticulum experiments][link-20].
* Scott Hogg examines [building an IPv6 training and testing lab][link-22].

## Security

* eBPF is taking hold in security exploits, like [the tool named "BPFdoor" that is penetrating critical infrastructure networks][link-2].
* Paul Meyer discusses [reproducing and mitigating BadAML][link-11], an ACPI-based attack vector that can be used to potentially gain access to confidential VMs. Meyer admits that the model in the article assumes the attacker has full control over the host, at which point I would assume "game over" and move on.
* This site provides not only [an illustrated TLS 1.2 connection diagram][link-15], but also offers diagrams for QUIC, DTLS, and TLS 1.3 (see the links in the upper right corner).
* Rory McCune continues [his series on unpatchable Kubernetes vulnerabilities][link-17].
* David Chisnall posted [a scathing analysis of Mythos][link-25] on Mastodon (hat tip to Bruce Davie for boosting it into my timeline), and shared a link to [an advisory on critical command injection vulnerabilities in Claude Code][link-26].

## Cloud Computing/Cloud Management

* I recently found [this alternative to LocalStack for AWS emulation][link-1].
* [How to hide regions and services in the AWS console][link-28] is a useful thing to know about.

## Operating Systems/Applications

* Chris Down [breaks down the differences between zswap and zram][link-3].
* Micah Kepe [introduces his new Rust-based JSON search tool][link-4], `jsongrep`, which purports to be faster than other JSON search tools.
* Christian Hofstede-Kuhn shares [some shell tricks that actually make life easier][link-5]. I really liked this line from the end of his post: "Stop letting the terminal push you around, and start rearranging the furniture. It’s your house now." Love it!
* The Unknown Universe (I couldn't find the author's actual name for attribution) is concerned that [the year of the Linux desktop might be a trap][link-7], thanks to pending age verification laws. All I can say is I am squarely in the Free Rebel camp.
* This Docker blog post on [assessing ARM64 readiness][link-21] points out an often overlooked issue: hard-coded dependencies that don't account for architecture.
* I know I am behind on this, but I have been investigating Docker versus Podman and I found this [in-depth comparison of both tools][link-24] to be helpful.

## Programming/Development

* [Your job isn't programming][link-6], according to Nick at Code and Cake. It's managing complexity.
* Working with a local LLM has been on my radar for some time now. With the new Linux PC build complete, I am ready to move forward---and I might just start by leveraging [this script to use Claude Code with a local LLM][link-8] (after I make sure I provide the workaround to the recently-disclosed Claude Code command injection vulnerability; see the "Security" section above).
* Nathaniel Fishel asserts that [your code is worthless][link-10] and that "vibe coding" and agentic AI programming techniques are "financing technical debt." I like this phrase Fishel uses because it's so true---technical debt, like real financial debt, has a cost and is paid over time (with interest).
* Sam Rose [explains bloom filters][link-12] (hat tip to David Flanagan).

## Storage

* When I came across this article stating that [Amazon S3 Files is still not a file system][link-14], I expected to find an argument supporting the title's claim. What I found was instead an explanation of S3 Files and how it presents a file system interface to S3---useful content, but not aligned with the article's title, in my opinion.

## Virtualization

* Emir Beganović shares their perspective on [the state of microVM isolation in 2026][link-16].
* Time will tell if Docker's decision to [build a new microVM virtual machine monitor (VMM) for Docker Sandboxes][link-27] was the right move. With so much activity in the microVM VMM space (see previous link!), it's a bit of a surprising move---but it sounds like the Windows and macOS support is what forced their hand.

## Career/Soft Skills

* Having recently taken a role as a manger leading a team of highly-skilled folks, I resonated with some of the things in this article on [seeing like a software company][link-9], which examines the concept of _legibility._
* Brian Merchant [explains why the AI backlash has turned violent][link-23]. No one deserves violence---I am glad that Merchant explicitly states this in the article---yet it's not hard to see why people feel threatened (and are getting defensive).

That's all I have for you today. I hope it was useful! Feel free to send interesting links you find my way---you can hit me up [on Mastodon][link-97], [on Bluesky][link-98], or [on X/Twitter][link-99] (although I use the latter infrequently). My email also isn't hard to find, and I love to hear from readers. Thanks for reading!

[link-1]: https://github.com/floci-io/floci
[link-2]: https://www.helpnetsecurity.com/2026/03/26/telecom-bpfdoor-detection-script/
[link-3]: https://chrisdown.name/2026/03/24/zswap-vs-zram-when-to-use-what.html
[link-4]: https://micahkepe.com/blog/jsongrep/
[link-5]: https://blog.hofstede.it/shell-tricks-that-actually-make-life-easier-and-save-your-sanity/
[link-6]: https://codeandcake.dev/posts/2025-12-12-your-job-isnt-programming
[link-7]: https://the.unknown-universe.co.uk/privacy-security/year-of-linux-trap/
[link-8]: https://www.xda-developers.com/wrote-script-run-claude-code-local-llm-skipping-cloud/
[link-9]: https://www.seangoedecke.com/seeing-like-a-software-company/
[link-10]: https://nathanielfishel.substack.com/p/your-code-is-worthless
[link-11]: https://katexochen.aro.bz/posts/badaml/
[link-12]: https://samwho.dev/bloom-filters
[link-13]: https://potsandpansbyccg.com/2026/04/10/hydrogen-generators/
[link-14]: https://dev.to/aws/amazon-s3-files-is-still-not-a-file-system-28g5
[link-15]: https://tls12.xargs.org/
[link-16]: https://emirb.github.io/blog/microvm-2026/
[link-17]: https://securitylabs.datadoghq.com/articles/unpatchable-kubernetes-vulnerabilities-cve-2020-8562/
[link-18]: https://reticulum.network/
[link-19]: https://tssmith.is/blog/sixthpost/
[link-20]: https://tssmith.is/blog/seventhpost/
[link-21]: https://www.docker.com/blog/how-to-analyze-hugging-face-for-arm64-readiness/
[link-22]: https://hoggnet.com/blogs/news/building-an-ipv6-training-and-testing-lab
[link-23]: https://www.bloodinthemachine.com/p/why-the-ai-backlash-has-turned-violent
[link-24]: https://dev.to/mechcloud_academy/docker-vs-podman-an-in-depth-comparison-2025-2eia
[link-25]: https://infosec.exchange/@david_chisnall/116419479557680376
[link-26]: https://beyondmachines.net/event_details/anthropic-claude-code-leak-reveals-critical-command-injection-vulnerabilities-e-6-c-1-k/gD2P6Ple2L
[link-27]: https://www.docker.com/blog/why-microvms-the-architecture-behind-docker-sandboxes/
[link-28]: https://dev.to/aws/hide-regions-and-services-in-the-aws-console-1m09
[link-97]: https://fosstodon.org/@scottslowe
[link-98]: https://bsky.app/profile/scottslowe.bsky.social
[link-99]: https://twitter.com/scott_lowe
