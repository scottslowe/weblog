---
author: slowe
categories: Information
comments: true
date: 2022-06-24T09:00:00-06:00
tags:
- Apple
- AWS
- Azure
- GitHub
- Hardware
- Kubernetes
- Linux
- macOS
- Networking
- Security
- Storage
- Tetragon
- Virtualization
title: 'Technology Short Take 156'
url: /2022/06/24/technology-short-take-156/
---

Welcome to Technology Short Take #156! It's been about a month since the last Technology Short Take, and in that time I've been gathering links that I wanted to share with my readers. (I still have quite the backlog of links to read!) Hopefully something I share here will prove useful to someone. Enjoy the links below, and enjoy your weekend!<!--more-->

## Networking

* I'd never heard of [Pipy][link-7] before seeing it in [this article][link-6], but it look like it could be quite useful for a number of use cases.
* William Morgan, one of the creators of [Linkerd][link-19], has [a lengthy treatise on eBPF, sidecars, and the future of the service mesh][link-18]. As a (relative) layperson---meaning I'm not an eBPF expert---I don't know if I should believe the eBPF cheerleaders (some of whom I know personally and are familiar with their technical expertise) or folks like William who have clearly "been there, done that" with service mesh. I certainly think there's a place for eBPF in service meshes, but I'm not yet on board with sidecar-less service meshes (or per-node proxy models).

## Security

* BPFDoor, as it is known, is a passive backdoor that allows threat actors to remotely connect to a Linux shell. Check out [this write-up][link-1]. Yikes!
* Think PDF files are safe? [Think again][link-2].
* Tetragon is Isovalent's newly open-sourced security framework. Read [more about it on the Isovalent blog][link-9].
* 9to5Mac outlines some details on [the so-called PACMAN attack against Apple's M1 chips][link-22]. I think the headline ("defeats 'the last line of security'") is a bit sensationalist, but I guess that's the world we live in.
* What I've seen of [the new Hertzbleed attack][link-24], on the other hand, perhaps isn't sensationalist enough. Ugh.
* Tzah Pahima [shares additional details][link-26] on the so-called Azure "SynLapse" security vulnerability.
* Akamai has [a post describing Panchan][link-27] (in their words, "a peer-to-peer botnet and SSH worm").

## Cloud Computing/Cloud Management

* Rob Salmond has a good article on [Prometheus service discovery, Istio, and mTLS][link-8].
* Scott Rosenberg has a piece on [how to learn Tanzu][link-12]. He didn't mention me in his section on Cluster API, but I've been known to write [a thing or two about Cluster API][link-13] (insert smiley emoji here).
* If you're interested in what's happening in the Knative space, you may find [this site][link-20] (that I recently discovered) useful.
* I also recently discovered Anaïs Urlichs' [weekly-but-not-so-weekly Seven-Day DevOps newsletter][link-21], which---like the rest of her site---has some great information.
* Kim Wuestkamp discusses [a change in Kubernetes 1.24 regarding ServiceAccounts and their Secrets][link-23].
* Mike Roberts shares [his view of the good, the bad, and the scary][link-28] regarding the AWS CDK. While his post specifically addresses the AWS CDK, I think that some of his conclusions could also be applicable to other "infrastructure-as-_actual_-code" tools like Pulumi. That doesn't mean these tools aren't worth exploring (I definitely believe they are), but you should go into it fully aware of the potential concerns.

## Operating Systems/Applications

* I recently learned that you can modify macOS' PAM configuration to use Touch ID for `sudo`. That's handy. Read [Dan Moren's article for details][link-17].

## Programming

* Thierry De Pauw lays out [the arguments against the use of feature branches in software development][link-10].
* It's beyond my current skill level in programming, but I did read Tim Bray's recent article on [making code faster][link-11] in an effort to pick up some "tips and tricks." The key takeaway for me: premature optimization is the root of all evil. At my current skill level, I think that's going to be a mantra I need to live by.
* Here are some nice "advanced beginner" [notes on WASM][link-25].
* Paul Swail shares [why he switched from AWS CodePipeline to GitHub Actions][link-29].

## Storage

* Although it isn't technically specific to storage, there are significant storage themes in this article on [what might happen if you lock yourself out of your digital life][link-14]. Scary stuff!

## Virtualization

* Here's [Chris Evans' take][link-3] on the Broadcom acquisition of VMware.
* Colin shares [a critical learning item][link-15] on using Terraform with VMware ESXi.
* Apple's Worldwide Developer Conference (WWDC) recently happened, and some virtualization-related information emerged on how Apple is allowing ARM-based Linux VMs (virtualized using the hypervisor present in macOS) to leverage Rosetta 2 to run Intel binaries. Ars Technica has more information [here][link-16]. (Personally, I think this is pretty cool, even if the setup process to make it work is like jumping through hoops.)

## Career/Soft Skills

* I liked [this brief article][link-4] on the true nature of remote work (hint: it's about working asynchronously.)
* Julien Simon's treatise on [technical evangelism from the trenches][link-5] was also a good read, with a number of tips that I'll be attempting to assimilate into my thinking and work.

That's all for now! I love hearing from readers, so if you feel like getting in touch with me there are a variety of ways to do that. I'm [on Twitter][link-99], in a variety of Slack channels, and you can even e-mail me (my e-mail address isn't too hard to find, just poke around on this site for a bit). Thanks for reading!

[link-1]: https://www.bleepingcomputer.com/news/security/bpfdoor-stealthy-linux-malware-bypasses-firewalls-for-remote-access/
[link-2]: https://www.bleepingcomputer.com/news/security/pdf-smuggles-microsoft-word-doc-to-drop-snake-keylogger-malware/
[link-3]: https://www.architecting.it/blog/broadcom-vmware/
[link-4]: https://mycodingtales.com/stop-pretending-your-company-is-remote/
[link-5]: https://julsimon.medium.com/technical-evangelism-from-the-trenches-b26ed0642709
[link-6]: https://opensource.com/article/22/5/pipy-programmable-network-proxy-cloud
[link-7]: https://flomesh.io/
[link-8]: https://superorbital.io/journal/istio-metrics-merging/
[link-9]: https://isovalent.com/blog/post/2022-05-16-tetragon
[link-10]: https://thinkinglabs.io/articles/2022/05/30/on-the-evilness-of-feature-branching-the-problems.html
[link-11]: https://www.tbray.org/ongoing/When/202x/2022/06/10/Quamina-Optimizing
[link-12]: https://www.vrabbi.cloud/post/how-to-learn-tanzu
[link-13]: /tags/capi
[link-14]: https://shkspr.mobi/blog/2022/06/ive-locked-myself-out-of-my-digital-life/
[link-15]: https://www.vgemba.net/vmware/terraform/terraform-vsphere-host-ssl/
[link-16]: https://arstechnica.com/gadgets/2022/06/macos-ventura-will-extend-rosetta-support-to-linux-virtual-machines/
[link-17]: https://sixcolors.com/post/2020/11/quick-tip-enable-touch-id-for-sudo/
[link-18]: https://buoyant.io/2022/06/07/ebpf-sidecars-and-the-future-of-the-service-mesh/
[link-19]: https://linkerd.io/
[link-20]: https://salaboy.com/
[link-21]: https://anaisurl.com/tag/devops/
[link-22]: https://9to5mac.com/2022/06/10/pacman-m1-chip/
[link-23]: https://itnext.io/big-change-in-k8s-1-24-about-serviceaccounts-and-their-secrets-4b909a4af4e0
[link-24]: https://www.hertzbleed.com/
[link-25]: http://neugierig.org/software/blog/2022/06/wasm-notes.html
[link-26]: https://orca.security/resources/blog/synlapse-critical-azure-synapse-analytics-service-vulnerability/
[link-27]: https://www.akamai.com/blog/security/new-p2p-botnet-panchan
[link-28]: https://blog.symphonia.io/posts/2022-06-07_cdk_good_bad_scary
[link-29]: https://serverlessfirst.com/switch-codepipeline-to-github-actions/
[link-99]: https://twitter.com/scott_lowe
