---
author: slowe
categories: Information
comments: true
date: 2026-05-08T09:00:00-05:00
tags:
- AWS
- Cisco
- ESXi
- Hardware
- Kubernetes
- Networking
- Security
- Terraform
- Virtualization
- VMware
title: Technology Short Take 195
url: 2026/05/08/technology-short-take-195
---

Welcome to Technology Short Take #195! It wasn't planned this way, but it seems like this Tech Short Take is heavily slanted toward AI/LLM-related articles and posts. Topics like security concerns around improper storage of API keys, how developers are using AI tools, spyware getting installed with AI assistants, and how AI/LLMs might be creating barriers to entry for new IT profesionals are all on tap this time around. I hope this unintentional focus doesn't prevent you from finding something useful!<!--more-->

## Networking

* Ivan Pepelnjak takes readers through the process of [generating partial devices configurations with `netlab`][link-19].
* It's an older blog post, but it checks out---have a look at this [walkthrough of Containerlab and Netlab][link-20]. (Hat tip to Ivan for the link. Also, bonus points if you understood the reference at the start of this paragraph.)
* Ah, MTU issues...[they don't go away][link-24] if you migrate to Kubernetes.

## Security

* Sean Gallagher and Omid Mirzaei from Cisco Talos discuss [how threat actors are misusing AI workflow automation][link-3].
* Before [this article on vulnerability triage][link-6], I'd never heard of "brocards."
* Kelby Ludwig reminds folks that [you don't want long-lived keys][link-11].
* Davi Ottenheimer [tackles the claims about risk from Anthropic's Mythos preview][link-12]. (Hint: the words "missing", "failed", "fluffy bunny", and "FUD" are involved.)
* William Collins reminds readers that they still need to pay attention to where they're [storing sensitive data like API keys][link-14].
* Copy Fail has been making the rounds on lots of articles and blogs. Clément Nussbaumer looks at Copy Fail from the perspective of [going from unprivileged Pod to root on a Kubernetes Node][link-22].
* Here's another one on Copy Fail, this time looking at [blocking Copy Fail with Tetragon][link-23].
* Security is an arms race, and trying to secure AI agents is no different. The latest round in the race is [a Falco project called Prempti][link-25].

## Cloud Computing/Cloud Management

* It's a good thing I came across this article about [why friends don't let friends use Ollama][link-1]. I had initially thought I'd use Ollama in some of my local LLM experiments, but now I know I'll stick to llama.cpp.
* Jorijn Schrijvershof takes a look at [Terraform vs. OpenTofu three years after the fork][link-2].
* Eric Johnson takes [a closer look at Lambda-managed instances][link-10] (running Lambda functions on EC2 instances of your choice in your account).

## Operating Systems/Applications

* On the heels of [Cal.com going closed source][link-4], Discourse [firmly pledges to stay open][link-5].
* What do the Turing test, Searle's Chinese room, and Chomsky's universal grammar have in common? According to Rodney Brooks, they are [three things that LLMs have made us have to rethink][link-8]. I was familiar with the Turing test, but the other two items were new to me.
* Alexander Hanff discovered that [Anthropic secretly installed spyware along with Claude Desktop][link-9].
* Eric Sloof shared a "beginner's tutorial" for [building a GPU lab for the NVIDIA NCA-AIIO exam][link-21]. Long-time readers probably recognize Eric's name as a stalwart within the virtualization community; it's good to see him branching out into other areas.

## Programming/Development

* Alisa Sireneva laments that [programming used to be free][link-7], and that the rise of LLM-assisted software development---and the associated costs or barriers to entry---might prevent others from following the same path she did.
* Are AI coding agents causing teams to [accumulate hidden technical debt][link-13]?
* Federico Paolinelli shares [how he's using AI in 2026][link-15].
* Using the Rust reimplementation of GNU coreutils as a framework, Matthias Endler looks at [some bugs Rust won't catch][link-17].

## Virtualization

* William Lam shares some news about [an AMD Zen4/Zen5 IPMI thermal driver for ESX][link-18].

## Career/Soft Skills

* [Does repeated, prolonged use of AI degrade cognitive ability?][link-16]

That is all I have for you today. If you come across an article you think would be a good candidate for inclusion in a future Tech Short Take, feel free to forward it my way. You can send it to me via email (my address is on this site and isn't terribly hard to find or figure out), or you can share it with me via social media. I am available on [Mastodon][link-97], [Bluesky][link-98], and [X/Twitter][link-99] (although I rarely check the latter). You can also contact me via Slack, as I am active in several different open source Slack communities. Thanks for reading!

[link-1]: https://sleepingrobots.com/dreams/stop-using-ollama/
[link-2]: https://jorijn.com/en/blog/opentofu-vs-terraform-2026-the-fork-finally-diverged/
[link-3]: https://blog.talosintelligence.com/the-n8n-n8mare/
[link-4]: https://cal.com/blog/cal-com-goes-closed-source-why
[link-5]: https://blog.discourse.org/2026/04/discourse-is-not-going-closed-source/
[link-6]: https://blog.yossarian.net/2026/04/11/Brocards-for-vulnerability-triage
[link-7]: https://purplesyringa.moe/blog/programming-used-to-be-free/
[link-8]: https://rodneybrooks.com/three-things-that-llms-have-made-us-rethink/
[link-9]: https://www.thatprivacyguy.com/blog/anthropic-spyware/
[link-10]: https://edjgeek.com/blog/lambda-managed-instances/
[link-11]: https://argemma.com/blog/long-lived-keys/
[link-12]: https://www.flyingpenguin.com/the-boy-that-cried-mythos-verification-is-collapsing-trust-in-anthropic/
[link-13]: https://blog.technodrone.cloud/2026/04/hidden-cost-of-ai.html
[link-14]: https://wcollins.io/posts/2026/your-mcp-config-is-leaking-secrets/
[link-15]: https://fedepaol.github.io/blog/2026/04/25/how-i-use-ai-in-2026/
[link-16]: https://ai-project-website.github.io/AI-assistance-reduces-persistence/
[link-17]: https://corrode.dev/blog/bugs-rust-wont-catch/
[link-18]: https://williamlam.com/2026/05/amd-zen4-zen5-ipmi-thermal-driver-for-esx-fling.html
[link-19]: https://blog.ipspace.net/2026/04/netlab-generate-device-configs/
[link-20]: https://theworldsgonemad.net/2025/lab-as-code-pt2/
[link-21]: https://www.ntpro.nl/blog/archives/3869-Build-a-GPU-Lab-for-the-NVIDIA-NCA-AIIO-Exam-with-RunPod-Beginner-Tutorial.html
[link-22]: https://clement.n8r.ch/en/articles/copyfail-cve-2026-31431-kubernetes-escape/
[link-23]: https://isala.me/blog/mitigating-copy-fail-with-tetragon/
[link-24]: https://lovable.dev/blog/hunting-networking-bugs-in-kubernetes
[link-25]: https://github.com/falcosecurity/prempti
[link-97]: https://fosstodon.org/@scottslowe
[link-98]: https://bsky.app/profile/scottslowe.bsky.social
[link-99]: https://twitter.com/scott_lowe
