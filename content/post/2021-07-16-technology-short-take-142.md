---
author: slowe
categories: Information
comments: true
date: 2021-07-16T13:25:00-06:00
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- Kubernetes
- AWS
- Envoy
- Etcd
- Kubernetes
- SSH
- Git
- CLI
- ESXi
- KVM
- Go
- HTML
title: 'Technology Short Take 142'
url: /2021/07/16/technology-short-take-142/
---

Welcome to Technology Short Take #142! This time around, the Networking section is a bit light, but I've got plenty of cloud computing links and articles for you to enjoy, along with some stuff on OSes and applications, programming, and soft skills. Hopefully there's something useful here for you!<!--more-->

## Networking

* Bill Doerrfeld discusses [the use of WebAssembly to extend the Envoy proxy][link-13] here after a discussion with Idit Levine, CEO and founder of Solo.io. Envoy is increasingly becoming an important component in many cloud-native networking solutions, so the ability to extend or customize Envoy's behavior is quite important in my opinion.

## Servers/Hardware

* Kevin Houston [speculates on the future of the blade server][link-23], wondering if higher-wattage CPUs mean the end of the blade server form factor.
* Is the introduction of [Google Cloud's new Tau VMs][link-24] an indication that AMD's influence continues to grow over Intel's?

## Security

* Andy Robbins discusses the concept of attack paths (specifically, identity-based attack paths) in [this post][link-1].
* Matt Fuller has authored a pretty comprehensive look at [the ways data can be shared across AWS accounts][link-3].
* Microsoft has released [clarified guidance for dealing with the recent Windows Print Spooler vulnerability][link-27].

## Cloud Computing/Cloud Management

* Etcd---the key-value store behind Kubernetes, among other things---saw it's 3.5 release recently; here's [the announcement blog post][link-6]. I was particularly impressed by some of the improvements in this release, such as the 50% reduction in memory consumption during peak usage and the reduction of the etcd code base by half. Great work!
* I found this two-part series on Kinesis data streams ([part 1][link-7], [part 2][link-8]) by Anahit Pogosova to be very informative. I was not familiar with AWS Kinesis Data Streams, but at least now I have a rough idea of what's involved. There's a ton of information in these two posts, so if this is a key technology for you I highly recommend giving this series a read.
* This is a good whitepaper on [organizing your AWS environment using multiple accounts][link-9].
* Sarah Wang and Martin Casado sparked quite the discussion with [this post on the cost of cloud computing][link-10]. I didn't find the conclusions of the article to be all that "shocking," but many folks did (the "TL;DR" is that you should consider/evaluate cloud/cost optimizations as you grow, which may include repatriation).
* It's neat to see the Kubernetes Ingress APIs evolving; you can get more information on the Kubernetes Gateway API [here][link-11]. [TGIK 148][link-12] with the inimitable Josh Rosso may also be useful.
* Chip Zoller looks at [using Kyverno for Kubernetes Custom Resources][link-17].

## Operating Systems/Applications

* It's pretty cool to see that GitHub is [now supporting security keys for SSH Git operations][link-2].
* I've made no secret of the fact I'm a fan of the `fzf` command-line tool. In case you want to "up" your `fzf` game, check out [this Pragmatic Pineapple post][link-14].
* Via Michael Tsai, here's an article by Rich Trouton on [using `curl` for Telnet testing][link-20]. What's that? You don't know what `telnet` is? See [here][link-21].
* This is [a handy `docker-compose` tip][link-29].
* And here's another [handy trick][link-30], courtesy of Maish Saidel-Keesing.
* Rounding out a trio of useful tips and tricks, Justin Garrison shows off [some GitHub URL hacks][link-31].

## Programming

* Here's [a nice collection of useful HTML tips][link-4].
* Alex Edwards [shows some examples][link-18] of using Go 1.16's `flag.Func()` function for custom command-line flags.
* Someday I hope to understand the wisdom hidden in [this post on writing code that is easy to delete, not easy to extend][link-25]. Right now, I don't have enough direct experience to be able to consume it.

## Virtualization

* Gabrie Van Zanten has a post on [a PowerShell script he wrote to find VM NUMA locality][link-16].
* The Project Zero team at Google [document][link-19] their findings on a stable guest-to-host escape when running KVM on AMD processors (the bug is in the AMD-specific code for KVM).
* William Lam [discusses ESXi on SimplyNUC Ruby and Topaz][link-22] and also provides [some information on building a Harbor virtual appliance][link-28].

## Career/Soft Skills

* Here's [a list of 21 hands-on AWS builders][link-5] you may want to consider following.
* Julia Evans has a rule to help with blogging: [blog about what you've struggled with][link-15] (hat tip to Ivan Pepelnjak for linking to Julia's post). It's a rule I've used in the past, and it's helpful. If you're thinking about blogging (it's not for everyone, but it _can_ be quite fulfilling), you might consider following this guideline.
* This article on [the cultural implications of silence][link-26] was a great read (hat tip to the Learning How to Learn community, which referred me to this article).

And that's all, folks! If you found this post useful, please feel free to share it via your social media accounts. If you have feedback---and I'm always open to constructive feedback---I'd love to hear from you! Feel free to contact [me on Twitter][link-99], find me on one of the Slack communities I frequent, or send me an e-mail (my address is not hard to find). Thanks for reading!

[link-1]: https://posts.specterops.io/the-attack-path-management-manifesto-3a3b117f5e5
[link-2]: https://github.blog/2021-05-10-security-keys-supported-ssh-git-operations/
[link-3]: https://matthewdf10.medium.com/aws-accounts-as-security-boundaries-97-ways-data-can-be-shared-across-accounts-b933ce9c837e
[link-4]: https://markodenic.com/html-tips/
[link-5]: https://acloudguru.com/blog/engineering/follow-the-builders-21-hands-on-aws-builders-to-follow-in-2021
[link-6]: https://etcd.io/blog/2021/announcing-etcd-3.5/
[link-7]: https://dev.solita.fi/2020/05/28/kinesis-streams-part-1.html
[link-8]: https://dev.solita.fi/2020/12/21/kinesis-streams-part-2.html
[link-9]: https://docs.aws.amazon.com/whitepapers/latest/organizing-your-aws-environment/organizing-your-aws-environment.html
[link-10]: https://a16z.com/2021/05/27/cost-of-cloud-paradox-market-cap-cloud-lifecycle-scale-growth-repatriation-optimization/
[link-11]: https://gateway-api.sigs.k8s.io
[link-12]: https://www.youtube.com/watch?v=W_P7DBnDikY&list=PL7bmigfV0EqQzxcNpmcdTJ9eFRPBe-iZa&index=10
[link-13]: https://containerjournal.com/features/extending-the-envoy-proxy-with-webassembly/
[link-14]: https://pragmaticpineapple.com/four-useful-fzf-tricks-for-your-terminal/
[link-15]: https://jvns.ca/blog/2021/05/24/blog-about-what-you-ve-struggled-with/
[link-16]: http://www.gabesvirtualworld.com/find-vm-numa-locality-with-powershell/
[link-17]: https://neonmirrors.net/post/2021-06/policy-k8s-customresources/
[link-18]: https://www.alexedwards.net/blog/custom-command-line-flags
[link-19]: https://googleprojectzero.blogspot.com/2021/06/an-epyc-escape-case-study-of-kvm.html
[link-20]: https://derflounder.wordpress.com/2021/05/23/using-curl-for-telnet-testing-on-macos-high-sierra-and-later/
[link-21]: https://en.wikipedia.org/wiki/Telnet
[link-22]: https://williamlam.com/2021/06/esxi-on-simplynuc-ruby-and-topaz.html
[link-23]: http://www.bladesmadesimple.com/2021/05/are-we-nearing-the-end-of-blade-servers/
[link-24]: https://cloud.google.com/blog/products/compute/google-cloud-introduces-tau-vms
[link-25]: https://programmingisterrible.com/post/139222674273/how-to-write-disposable-code-in-large-systems
[link-26]: https://www.rw-3.com/blog/cultural-implications-of-silence
[link-27]: https://msrc-blog.microsoft.com/2021/07/08/clarified-guidance-for-cve-2021-34527-windows-print-spooler-vulnerability/
[link-28]: https://williamlam.com/2021/07/packer-reference-for-vmware-harbor-virtual-appliance.html
[link-29]: https://nickjanetakis.com/blog/docker-tip-87-run-multiple-docker-compose-files-with-the-f-flag
[link-30]: https://blog.technodrone.cloud/2021/07/demo-sensitive-info-terminal.html
[link-31]: https://www.justingarrison.com/blog/2021-07-11-github-url-hacks/
[link-99]: https://twitter.com/scott_lowe
