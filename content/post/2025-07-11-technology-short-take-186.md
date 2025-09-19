---
author: slowe
categories: Information
comments: true
date: 2025-07-11T10:00:00-04:00
tags:
- AWS
- EKS
- Hardware
- IPv6
- Linux
- Microsoft
- Networking
- Security
- Storage
- Virtualization
- Windows
title: 'Technology Short Take 186'
url: /2025/07/11/technology-short-take-186/
---

Welcome to Technology Short Take #186! Yes, it's been quite a while since I published a Technology Short Take; life has "gotten in the way," so to speak, of gathering links to share with all of you. However, I think this crazy phase of my life is about to start settling down (I hope so, anyway), and I'm cautiously optimistic that I'll be able to pick up the blogging pace once again. For now, though, here's a collection of links I've gathered since the last Technology Short Take. I hope you find something useful here!<!--more-->

## Networking

* Nick Buraglio explains [how to use Cloudflare Tunnel for IPv6-only connectivity][link-5].
* A colleague recently introduced me to [Ultra Ethernet][link-9], which is (as I understand it) designed for AI and HPC workloads and intends to "close the gap" between InfiniBand and Ethernet. Although, in reading [this comparison between InfiniBand and Ethernet by WWT][link-10], it looks like the gap isn't necessarily all that large. (Hat tip to Drew for the links.)

## Security

* CSO Online briefly touches on [the scope of the Salt Typhoon hacking campaign][link-2], now revealed to include even more companies than previously thought.
* [161 security vulnerabilities, including three actively exploited zero-day vulnerabilities][link-7]? Holy broken software! I know that _no_ software is without security vulnerabilities, but this is just...wow. Be sure to read the linked article for more details on all the patched vulnerabilities to make sure you are appropriately protected.
* Here's an article describing [how to bypass disk encryption on systems with automatic TPM2 unlock][link-8].

## Cloud Computing/Cloud Management

* There's some good real-world experience contained in Jon Spriggs' post about [a few weird issues in his custom AWS EKS workers][link-3]. I love that Jon also included the workarounds for each issue; this is really helpful.

## Operating Systems/Applications

* Matt of NSHipster explains how to use the 1Password CLI (`op`) to [automatically inject secrets into environment files][link-1] in conjunction with `op run`.
* I appreciate Julia Evans' post on [what's involved in getting a "modern" terminal setup][link-4]. Julia's approach is `fish` plus `neovim` plus a terminal emulator with 24-bit color support. While the autocomplete suggestions from `fish` look really appealing, I've settled into `bash` plus `starship` plus `exa` (or `eza`) and a handful of very useful other CLI tools (`rg`, `fd`, and `fzf`, notably).
* It's no secret that I have not (thus far) been a fan of LLMs; I suppose there's just too much hype and not enough reality for my tastes. In spite of that---or perhaps because of that?---I appreciated David Crawshaw's balanced and down to earth discussion of [his personal experience using generative models while programming][link-6]. I particularly appreciated David's very honest acknowledgement of some of the flaws of LLMs, as well as how he shared both the good and the bad he's uncovered in his experimentation.
* I'll be honest, I love seeing stuff like [these anti-LLM scraping tools][link-11] start to emerge.
* What's this---someone [speaking _in favor_ of systemd][link-14]? Unthinkable!

That's all for now! I know, this one is shorter than the typical Technology Short Take. Nevertheless, hopefully I shared something useful. If you have any feedback, please feel free to reach out! I don't use Twitter/X very much these days, but I'm still [there][link-99]; you can also reach me [on Mastodon][link-29], [on Bluesky][link-30], or through one of the various Slack communities I frequent (such as [the Kubernetes Slack instance][link-28]). Thanks for reading!

[link-1]: https://nshipster.com/1password-cli/
[link-2]: https://www.csoonline.com/article/3632044/more-telecom-firms-were-breached-by-chinese-hackers-than-previously-reported.html
[link-3]: https://jon.sprig.gs/blog/post/8078
[link-4]: https://jvns.ca/blog/2025/01/11/getting-a-modern-terminal-setup/
[link-5]: https://www.forwardingplane.net/post/2025-01-11-cloudflared-tunnel-ipv6/
[link-6]: https://crawshaw.io/blog/programming-with-llms
[link-7]: https://krebsonsecurity.com/2025/01/microsoft-happy-2025-heres-161-security-updates/
[link-8]: https://oddlama.org/blog/bypassing-disk-encryption-with-tpm2-unlock/
[link-9]: https://ultraethernet.org/
[link-10]: https://www.wwt.com/blog/the-battle-of-ai-networking-ethernet-vs-infiniband
[link-11]: https://marcusb.org/hacks/quixotic.html
[link-14]: https://blog.tjll.net/the-systemd-revolution-has-been-a-success/
[link-28]: https://kubernetes.slack.com/
[link-29]: https://fosstodon.org/@scottslowe
[link-30]: https://bsky.app/profile/scottslowe.bsky.social
[link-99]: https://twitter.com/scott_lowe
