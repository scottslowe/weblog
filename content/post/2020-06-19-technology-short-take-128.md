---
author: slowe
categories: Information
comments: true
date: 2020-06-19T11:00:00-07:00
tags:
- Automation
- AWS
- Azure
- CLI
- Docker
- Encryption
- Git
- Hardware
- Intel
- Kubernetes
- Linux
- Apple
- macOS
- Networking
- NSX
- Pulumi
- Security
- Storage
- VirtualBox
- Virtualization
title: 'Technology Short Take 128'
url: /2020/06/19/technology-short-take-128/
---

Welcome to Technology Short Take #128! It looks like I'm settling into a roughly monthly cadence with the Technology Short Takes. This time around, I've got a (hopefully) interesting collection of links. The collection seems a tad heavier than normal in the hardware and security sections, probably due to new exploits discovered in Intel's speculative execution functionality. In any case, here's what I've gathered for you. Enjoy!<!--more-->

## Networking

* Graham Smith writes about [switching his NSX-T lab to use single N-DVS Edge nodes][link-1].
* Here's another useful article from Eric Sloof on [bridging between GNS3 and NSX-T][link-2]. Lots of network engineers are familiar with GNS3, and showing how to integrate it with NSX-T for testing and simulation is, I think, going to be helpful.
* The inimitable Ivan Pepelnjak provides [a primer on Azure networking][link-5].
* Sebastian Cohnen of StormForger shares [some results from network performance testing of AWS Fargate][link-21]. Note that this article is a couple of years old, so some things may have changed since it was written.
* Daniel Teycheney discusses [starting your network automation journey][link-28].

## Servers/Hardware

* [This article][link-3] from Carlos Fenollosa talks about his experience with a new 2020 MacBook Pro compared to his 2013-era MacBook Air. While there is some discussion of software in here (given Apple's tight coupling it can't be avoided), the article mostly focuses on hardware aspects of Apple's latest lineup of laptops. Personally, I appreciated Carlos' balanced review.
* This is one of those times when an article belongs in multiple categories---in this case, hardware and security. [This article][link-22] describes a new attack that "breaches the security guarantees" of Intel SGX.
* Oh, and while we're talking about Intel CPU exploits, [here's another one][link-23].

## Security

* [This article][link-7] explains how to use a "stacked filesystem" encryption solution named eCryptfs to encrypt directories on Linux. If you're interested in doing something like this for cloud storage, [this other article][link-8] shows how to use encryption with `rclone`.
* ZDNet covered [the news][link-12] of Check Point fixing a well-known memory corruption security hole in the GNU C Library.
* If you say that Linux malware doesn't exist...read [this article][link-17].
* The Citizen Lab [uncovers Dark Basin][link-24], described in their words as a "massive hack-for-hire operation."

## Cloud Computing/Cloud Management

* Michael McCune shares a look at [debugging the Kubernetes autoscaler using Cluster API and Docker][link-14].
* Jean-Paul Smit takes a look at Pulumi in the context of using it with C# to deploy Azure API Management. Read [his blog post here][link-15].
* Stellios Williams shares [how to download a ZIP of a private GitHub repository via the GitHub API][link-27] (something he's doing inside a Lambda function as part of automatic deployments of his Hugo-based site).

## Operating Systems/Applications

* Patrick Kremer shares a workaround for [DNS resolution for home labs when using corporate VPNs][link-4].
* Here's [a short list of "new generation" command-line tools][link-6]. I've adopted a couple of these myself (`ripgrep` and `bat`).
* Allan Odgaard (of TextMate fame) [slams macOS "Catalina"][link-11] on the basis of performance.
* Want to run Linux on your Surface Pro 4? [You're in luck.][link-13]
* Nick Gerace [shares why he uses Fedora][link-16].
* Chip Zoller shares his [GitHub Actions-based CI/CD for Hugo on AWS][link-18]. Building a pipeline for automatic deploys of this site is something I've been talking about doing for a while, so Chip's article was useful. I also learned about [the `s5cmd` utility][link-19], though for now I think I'll stick with [`s3deploy`][link-20].
* If you're thinking of renaming the "master" branch of your Git repositories to something else, Scott Hanselman [shows][link-25] how easy it can be.

## Storage

* Cormac Hogan takes a look at [using "manually" or "statically" configured Persistent Volumes with Cloud Native Storage (CNS) on vSphere][link-26].

## Virtualization

* JJ Asghar walks through [running Fedora CoreOS on VirtualBox][link-9].

## Career/Soft Skills

* Kyle Galbraith talks about [how communication and curiosity can matter more than technical skills][link-10] when it comes to a successful career in technology.

OK, that's all for now. As always, I'd love to hear from you, so feel free to find [me on Twitter][link-99] and let me know what you think of these posts. If you have suggestions for improvement, I'd love to hear those, too!

[link-1]: https://vdives.com/2020/06/01/nsx-t-3-0-lab-single-n-vds-edge-nodes/
[link-2]: https://www.ntpro.nl/blog/archives/3576-Bridging-between-GNS3-and-NSX-T.html
[link-3]: https://cfenollosa.com/blog/seven-years-later-i-bought-a-new-macbook-for-the-first-time-i-dont-love-it.html
[link-4]: http://www.patrickkremer.com/per-zone-dns-resolution-for-homelabs/
[link-5]: https://blog.ipspace.net/2020/05/azure-networking-101.html
[link-6]: https://kushaldas.in/posts/a-few-new-generation-command-line-tools.html
[link-7]: https://www.ostechnix.com/how-to-encrypt-directories-with-ecryptfs-in-linux/
[link-8]: https://www.linuxuprising.com/2020/05/how-to-encrypt-cloud-storage-files-with.html
[link-9]: https://jjasghar.github.io/blog/2020/05/26/fedora-coreos-working-on-virtualbox/
[link-10]: https://blog.kylegalbraith.com/2019/04/11/technical-skills-are-great-but-communication-and-curiosity-are-better/
[link-11]: https://sigpipe.macromates.com/2020/macos-catalina-slow-by-design/
[link-12]: https://www.zdnet.com/article/check-point-released-an-open-source-fix-for-common-linux-memory-corruption-security-hole/
[link-13]: https://steinwachs.net/blog/dualboot-windows-fedora-surfacepro4/
[link-14]: https://notes.elmiko.dev/2020/05/22/kubernetes-autoscaler-capd.html
[link-15]: https://www.jeanpaulsmit.com/2020/04/pulumi-deploy-apim/
[link-16]: https://nickgerace.dev/post/why-i-use-fedora
[link-17]: https://www.darkreading.com/vulnerabilities---threats/new-tycoon-ransomware-strain-targets-windows-linux/d/d-id/1338006
[link-18]: https://neonmirrors.net/post/2020-05/cicd-for-hugo-on-aws/
[link-19]: https://github.com/peak/s5cmd
[link-20]: https://github.com/bep/s3deploy
[link-21]: https://stormforger.com/blog/aws-fargate-network-performance/
[link-22]: https://www.bleepingcomputer.com/news/security/new-sgaxe-attack-steals-protected-data-from-intel-sgx-enclaves/
[link-23]: https://www.vusec.net/projects/crosstalk/
[link-24]: https://citizenlab.ca/2020/06/dark-basin-uncovering-a-massive-hack-for-hire-operation/
[link-25]: https://www.hanselman.com/blog/EasilyRenameYourGitDefaultBranchFromMasterToMain.aspx
[link-26]: https://cormachogan.com/2020/06/16/static-persistent-volumes-and-cloud-native-storage/
[link-27]: https://www.funkycloudmedina.com/2020/06/how-to-download-a-private-github-repository-zip-via-api/
[link-28]: https://blog.danielteycheney.com/posts/starting-your-network-automation-journey/
[link-99]: https://twitter.com/scott_lowe
