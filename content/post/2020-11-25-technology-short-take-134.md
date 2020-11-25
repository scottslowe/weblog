---
author: slowe
categories: Information
comments: true
date: 2020-11-25T08:15:00-07:00
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- Cilium
- Docker
- DNS
- AWS
- Linux
- Kubernetes
- macOS
- Istio
title: 'Technology Short Take 134'
url: /2020/11/25/technology-short-take-134/
---

Welcome to Technology Short Take #134! I'm publishing a bit early this time due to the Thanksgiving holiday in the US. So, for all my US readers, here's some content to peruse while enjoying some turkey (or whatever you're having this year). For my international readers, here's some content to peruse while enjoying dramatically lower volumes of e-mail because the US is on holiday. See, something for everyone!<!--more-->

## Networking

* Isovalent---the company behind Cilium, the eBPF-powered networking solution for Kubernetes clusters---just [launched with a $29M Series A round of funding][link-11]. Cilium is good stuff, and the Isovalent team is chock-full of great people. Congrats, Isovalent team!
* Has the time come for [the return of DNS cache poisoning attacks][link-33]?

## Security

* I'm glad to see [this][link-24]. Open source has become so critical to so many aspects of our computing infrastructure.
* [OpenCSPM][link-25] looks like it could be quite a useful tool. I haven't yet had time to dig in and get familiar with the details, but what I have seen so far looks good.
* Uh oh...[more hardware exploits][link-27].
* The macOS OCSP fiasco generated quite a bit of attention and analysis (see [here][link-29] and [here][link-30]).

## Cloud Computing/Cloud Management

* Shray Kumar provides [ten tips for running Istio in production][link-2]. This article is chock-full of useful information that, typically, can only be gathered from direct experience (like the note on using `istioctl` instead of the Istio Operator to avoid an issue when upgrading from 1.6 to 1.7).
* AWS has some [advice for customers dealing with Docker Hub rate limits][link-5]. I'm still somewhat in shock at the move by Docker to simply give away their position as the _de facto_ "app store" of the container world, but hey, what do I know? The void left by their decision is rapidly going to be filled by someone else (and based on this article it looks like AWS is gunning for the spot).
* Vlad Holubiev has [some tricks for making smaller Lambda artifacts and thus reducing cold start latency][link-7].
* Teammate Eric Shanks shows folks [how to use `ytt` to customize a TKG deployment][link-9]. It's a bit of a whirlwind introduction, but Eric also supplies some practical use cases/examples towards the end of the article. I often find that helpful when learning something new.
* I recently stumbled across Ricardo Sueiras' "AWS open source news and updates" posts. [Here's number 43][link-10], from 9 November 2020. Good stuff here!
* I wasn't aware of Cloudflare Argo Tunnel, so [this article][link-12] was informative for that reason alone. Throw in some Fargate and there's even more to learn!
* I predict we'll see more of [this kind of content][link-19] as folks seek to migrate away from Docker Hub.
* Niko Virtala shares some information on [making AWS developer tools work with AWS SSO][link-20].
* Adrian Hornsby shows [how to use AWS Systems Manager Automation to perform chaos engineering experiments][link-22]. I particularly appreciated how Adrian provides an example of how it's done, and walks readers through in detail how the example works. Adrian's [operational review readiness template][link-23] is another great post as well.
* The race to make Kubernetes ever smaller [continues][link-26].

## Operating Systems/Applications

* One of the things I love about Matt Oswalt is that he exemplifies the idea of a perpetual learner. The latest example is Matt's post on [the anatomy of a binary executable][link-3], in which he dives deep into what _exactly_ it means to be a binary executable file. Good stuff!
* You may have heard of eBPF, the Linux technology that is reshaping Linux applications (and in some ways reshaping Linux itself). Brendan Gregg [discusses the future of BPF binaries, made possible through BTF and CO-RE][link-4]. The idea of creating ELF binaries (don't know what that is? See the previous bullet!) for BPF is pretty cool, in my opinion, and has the potential to unlock a lot of innovation in this space.
* Something about Linux, and Fedora in particular, just keeps drawing me back. If you're in a similar boat, and you're looking for information on [how to get Firefox on Fedora to play H.264 videos][link-6], Leo Chavez has some information that should help.
* Here's [a bit of history][link-18] on macOS, for those of us interested in such things.
* [This][link-21] looks horribly confusing. It almost feels like we are well into Windows Registry territory here.
* What a time to be alive: [Microsoft has its own Linux distribution][link-28].
* Whether it be dissatisfaction with macOS 11 "Big Sur," or unhappiness at the direction of their hardware (there's some discussion that the new M1 chips don't support eGPUs), or concerns over privacy given the recent issues with OCSP and macOS "dialing home," I'm seeing folks leaving macOS for other platforms (mostly Linux). Preslav Rachev shares his story [here][link-31], and Juan Diego Caballero shares his story [here][link-32].

## Storage

* Chris Mellor shares [an interview with Hazelcast on the performance of Intel Optane PMem][link-34].

## Virtualization

* Mark Brookfield has an in-depth article on [using continuous deployment to provision VDI desktops][link-1]. The article covers everything from using Packer to automate the creation of the desktop images, to using HashiCorp Vault to store the credentials used in the desktop image, to using PowerCLI to recompose the VDI pool---all orchestrated by GitLab CI/CD. Well done.
* William Lam shares [how to build a complete vSphere with Tanzu homelab in just 32GB of RAM][link-8].
* VMware finally [splits VMware Tools out from vSphere][link-13].
* I told you AWS Nitro Enclaves were going to be interesting. In [this article][link-14], we see an example of using Nitro Enclaves to provide ultra-secure password storage within EC2 instances. The author of the article, Luc van Donkersgoed, also has a couple of articles on using Nitro Enclaves for ACM (see [part 1][link-15] and [part 2][link-16]).
* Frank Denneman has a great article that [walks readers through the network configuration for vSphere with Tanzu][link-17], helping to break down each of the various networks and IP addresses required.
* Blake Garner shares [a Packer template for macOS 11 on VMware Fusion][link-35].

That's all this time, but hopefully it's enough! If you have suggestions for content to include in a future Technology Short Take, or if you'd just like to catch up and say hello, feel free to contact [me on Twitter][link-99]. Enjoy the rest of your week!

[link-1]: https://virtualhobbit.com/2020/11/06/using-continuous-deployment-to-provision-vdi-desktops/
[link-2]: https://itnext.io/ten-tips-for-running-istio-in-production-4ea2b158440a
[link-3]: https://oswalt.dev/2020/11/anatomy-of-a-binary-executable/
[link-4]: http://www.brendangregg.com/blog/2020-11-04/bpf-co-re-btf-libbpf.html
[link-5]: https://aws.amazon.com/blogs/containers/advice-for-customers-dealing-with-docker-hub-rate-limits-and-a-coming-soon-announcement/
[link-6]: https://leochavez.org/index.php/2020/10/12/fedora-firefox-and-h264/
[link-7]: https://itnext.io/3x-smaller-lambda-artifacts-by-removing-junk-from-node-modules-2b50780ca1f5
[link-8]: https://www.virtuallyghetto.com/2020/11/complete-vsphere-with-tanzu-homelab-with-just-32gb-of-memory.html
[link-9]: https://theithollow.com/2020/11/09/using-ytt-to-customize-tkg-deployments/
[link-10]: https://blog.beachgeek.co.uk/posts/aws-open-source-news-and-updates-no-43-2jba/
[link-11]: http://www.globenewswire.com/news-release/2020/11/10/2124062/0/en/Isovalent-Launches-Breakthrough-New-Approach-to-Cloud-Native-Networking-With-29-Million-Series-A-Led-by-Andreessen-Horowitz-and-Google.html
[link-12]: https://www.obytes.com/blog/setting-up-a-cloudflare-argo-tunnel-on-aws-fargate
[link-13]: https://blogs.vmware.com/vsphere/2020/11/vmware-tools-is-now-its-own-product.html
[link-14]: https://www.sentiatechblog.com/ultra-secure-password-storage-with-nitropepper
[link-15]: https://www.sentiatechblog.com/acm-for-nitro-enclaves-its-a-big-deal
[link-16]: https://www.sentiatechblog.com/acm-for-nitro-enclaves-how-secure-are-they
[link-17]: https://frankdenneman.nl/2020/11/06/vsphere-with-tanzu-vcenter-server-network-configuration-overview/
[link-18]: https://www.howtogeek.com/698532/before-mac-os-x-what-was-nextstep-and-why-did-people-love-it/
[link-19]: https://cloud.google.com/blog/topics/developers-practitioners/hack-your-own-custom-domains-container-registry
[link-20]: https://www.cloudgardener.dev/how-to-use-aws-developer-tools-with-aws-sso/
[link-21]: https://eclecticlight.co/2020/11/09/wrangling-file-paths-in-catalina-and-big-sur/
[link-22]: https://adhorn.medium.com/creating-your-own-chaos-monkey-with-aws-systems-manager-automation-6ad2b06acf20
[link-23]: https://adhorn.medium.com/operational-readiness-review-template-e23a4bfd8d79
[link-24]: https://openssf.org/
[link-25]: https://darkbit.io/blog/announcing-opencspm
[link-26]: https://medium.com/@adamparco/announcing-k0s-the-smallest-simplest-kubernetes-distribution-3626c86575d5
[link-27]: https://zt-chen.github.io/voltpillager/
[link-28]: https://www.infoworld.com/article/3596347/microsoft-adds-a-new-linux-cbl-mariner.html
[link-29]: https://sneak.berlin/20201112/your-computer-isnt-yours/
[link-30]: https://blog.cryptohack.org/macos-ocsp-disaster
[link-31]: https://preslav.me/2020/11/16/farewell-macos-ubuntu/
[link-32]: https://monadical.com/posts/moving-to-linux-desktop.html#
[link-33]: https://thehackernews.com/2020/11/sad-dns-new-flaws-re-enable-dns-cache.html
[link-34]: https://blocksandfiles.com/2020/11/18/hazelcast-in-memory-hardware-interview/
[link-35]: https://netjibbing.com/post/packer-macos-11/
[link-99]: https://twitter.com/scott_lowe
