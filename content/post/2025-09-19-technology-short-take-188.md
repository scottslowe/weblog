---
author: slowe
categories: Information
comments: true
date: 2025-09-19T09:00:00-05:00
tags:
- AWS
- GitHub
- Go
- Hardware
- Linux
- macOS
- Networking
- OPA
- Security
- Storage
- Virtualization
- VMware
title: 'Technology Short Take 188'
url: /2025/09/19/technology-short-take-188/
---

Welcome to Technology Short Take #188! I'm back once again with a small collection of articles and links related to a variety of data center-related technologies. I hope you find something useful!

## Networking

* Scott Hogg discusses [using dual-stack network configurations on your DNS name servers][link-9] as part of an overall IPv6 deployment plan.
* Leon Adato explains why [knowing what's under the hood helps][link-14] when it comes to networking telemetry.
* Via Ivan Pepelnjak, I found [Suresh Vina's article on using netlab to build network labs][link-22].

## Security

* The use of malicious software packages continues to be a vector for attacks; here's [the latest discovery of malicious Go and `npm` packages][link-1].
* James Kettle explains [why HTTP/1.1 must die][link-4].
* Gary Marcus and Nathan Hamiel discuss [why the combination of LLMs plus coding agents has the potential to result in a security nightmare][link-6].
* [Weaponized RAR files are delivering a backdoor to Linux systems][link-12] using only a filename---more details in the linked article.
* [WhatsApp patches a zero-click exploit targeting iOS and macOS devices][link-16]. I don't use WhatsApp, but I know lots of folks that do (it's especially popular among my international friends). If you're a WhatsApp user, be sure to update.
* More software supply chain attacks, this time [targeting CrowdStrike's `npm` packages][link-20], have been observed as part of a larger campaign known as "Shai-Hulud."

## Cloud Computing/Cloud Management

* Mike Roberts explains why [you probably don't want to use AWS' new GitHub Actions Lambda Deployment action][link-2].
* What would you do if [AWS deleted your account and all your data][link-21]?
* AWS has _finally_ released [universal macOS installers for the AWS CLI][link-23].

## Operating Systems/Applications

* Adam Thompson shares [how to improve Logitech MX Master 3 mouse support on Arch Linux][link-3].
* Mitchell Hashimoto shares some lessons learned when they [rewrote the Ghostty GTK application][link-5].
* [Some of the creators of Open Policy Agent (OPA) are joining Apple.][link-7] Interesting.
* Simon Späti shares details on their [journey from macOS to Arch Linux with Omarchy][link-13].
* Here is a useful post on [systemd service hardening][link-17].
* The details of [this post on Apple's new Memory Integrity Enforcement (MIE)][link-18] are a bit beyond me, I'll be honest. I'm of two minds on the new technology. On one hand, it's nice to see progress at fixing memory safety-related bugs and exploits. On the other hand, this progress could only be achieved, as the article states, through combining "the unique strengths of Apple silicon hardware with our advanced operating system security." In other words, more tightly coupling the hardware and the OS.

## Programming/Development

* Go has [changed the way the language handles `GOMAXPROCS` in the 1.25 release][link-10].

## Virtualization

* In advance of VMware Explore 2025---which apparently is/was this week in Las Vegas---William Lam shares [an update on the nested ESXi 8.0 and ESXi 9.0 virtual appliance][link-11].

## Career/Soft Skills

* Ashley Willis talks about [the value and importance of the quiet season][link-8]. I particularly found this sentence applicable to my own life: "So yes, I'm writing again. And maybe I'm less 'everywhere' than I used to be. But I'd rather show up less often and have something worth saying than burn myself out trying to convince the world I'm still here." Well said, IMO.
* [All too true.][link-15]
* I don't know if I would go so far as to say [I am an AI hater][link-19], but I am most definitely opposed to AI in its current form. (I don't even like calling it "AI" when it's really nothing more than a statistical model for putting words together. But that's another discussion for another day...) As the author says, being a hater is a "kind of integrity" all its own---so I say, if you're an AI hater, don't be afraid to say so.

It's time to wrap up now---but don't be sad, I'll be back soon with more content. In the meantime, if you'd like to reach out to me to provide some feedback on this article or any article, I'd love to hear from you! Feel free to contact me via [Bluesky][link-29], via [Mastodon][link-30], via [X/Twitter][link-99], via Slack (I frequent a number of different communities, including the Kubernetes Slack instance) or even via email. Thanks for reading!

[link-1]: https://thehackernews.com/2025/08/malicious-go-npm-packages-deliver-cross.html
[link-2]: https://blog.symphonia.io/posts/2025-08-08_aws_lambda_deploy_github_action
[link-3]: https://hackeradam.com/improve-logitech-mx-master-3-mouse-support-on-arch-linux/
[link-4]: https://portswigger.net/research/http1-must-die
[link-5]: https://mitchellh.com/writing/ghostty-gtk-rewrite
[link-6]: https://garymarcus.substack.com/p/llms-coding-agents-security-nightmare
[link-7]: https://blog.openpolicyagent.org/note-from-teemu-tim-and-torin-to-the-open-policy-agent-community-2dbbfe494371
[link-8]: https://ashley.dev/posts/the-quiet-season/
[link-9]: https://hoggnet.com/blogs/news/why-you-should-dual-stack-your-dns-nameservers
[link-10]: https://go.dev/blog/container-aware-gomaxprocs
[link-11]: https://williamlam.com/2025/08/updated-nested-esxi-8-x-9-0-virtual-appliance.html
[link-12]: https://gbhackers.com/stealth-threat-unpacked-weaponized-rar-files/
[link-13]: https://www.ssp.sh/blog/macbook-to-arch-linux-omarchy/
[link-14]: https://www.adatosystems.com/2025/08/11/knowing-whats-under-the-hood-helps/
[link-15]: https://churchofturing.github.io/the-enterprise-experience.html
[link-16]: https://thehackernews.com/2025/08/whatsapp-issues-emergency-update-for.html
[link-17]: https://roguesecurity.dev/blog/systemd-hardening
[link-18]: https://security.apple.com/blog/memory-integrity-enforcement/
[link-19]: https://anthonymoser.github.io/writing/ai/haterdom/2025/08/26/i-am-an-ai-hater.html
[link-20]: https://socket.dev/blog/ongoing-supply-chain-attack-targets-crowdstrike-npm-packages
[link-21]: https://www.seuros.com/blog/aws-deleted-my-10-year-account-without-warning/
[link-22]: https://www.packetswitch.co.uk/netlab-the-fastest-way-to-build-network-labs/
[link-23]: https://aws.amazon.com/blogs/devops/introducing-universal-installers-for-aws-cli-v2-on-macos/
[link-29]: https://bsky.app/profile/scottslowe.bsky.social
[link-30]: https://fosstodon.org/@scottslowe
[link-99]: https://twitter.com/scott_lowe
