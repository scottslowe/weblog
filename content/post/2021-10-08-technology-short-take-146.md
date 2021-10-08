---
author: slowe
categories: Information
comments: true
date: 2021-10-08T09:00:00-06:00
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- DNS
- Cisco
- NSX
- VMware
- Kubernetes
- OPA
- AWS
- Linux
- vSphere
title: 'Technology Short Take 146'
url: /2021/10/08/technology-short-take-146/
---

Welcome to Technology Short Take #146! Over the last couple of weeks, I've gathered a few technology-related links for you all. There's some networking stuff, a few security links, and even a hardware-related article. But enough with the introduction---let's get into the content!<!--more-->

## Networking

* Denise Fishburne has [a 7-part series on IPv6][link-5].
* Alec Muffett writes about [DNS over HTTP over Tor][link-6], affectionately called DoHoT. As Alec shares in the article, it turns out that adding Tor to a "standard" DoH configuration doesn't significantly impact performance (as measured by latency).
* Julia Evans [recently released a tool named `dnspeep`][link-12] to help folks understand how DNS works.
* Russ White shares [Russ' rules for network design][link-13]. I also really enjoyed [his post on Keith's Law][link-16].

## Servers/Hardware

* Chris Mellor [speculates][link-3] that Cisco UCS may be on the way out; Kevin Houston [responds][link-4] with a "I don't think so." Who will be correct? I guess we will just have to wait and see.

## Security

* Jimmy Mankowitz talks in some detail about [using NSX-T in vSphere with Tanzu to secure applications][link-1].
* Here's a conveniently compiled [list of container CVEs][link-7].
* Lars Larsson discusses [hardening Kubernetes beyond the NSA and CISA guidance][link-19].

## Cloud Computing/Cloud Management

* Tom Kravitz' article on [how IBM lost the public cloud war][link-8] is a fascinating look at the internals of a former technology powerhouse. (Hat tip to Corey Quinn for sharing this on Twitter and bringing it to my attention.)
* AWS recently [announced the Cloud Control API][link-10]. Pulumi wasted no time in building their [native AWS provider][link-11] on top of the new API.
* The ARM takeover continues---AWS Lambda functions [can now execute on their Graviton2 processors][link-14].
* I love [this post][link-15].
* Kurt Roekle takes a second look at [combining Open Policy Agent with Kong Mesh][link-18], looking at the potential benefits offered by including Styra Declarative Authorization Service (DAS).
* Abhinav Sonkar walks through [using Argo CD's ApplicationSet functionality][link-21].

## Operating Systems/Applications

* Vivek Gite has instructions on [how to install `ffmepg` with NVIDIA GPU acceleration][link-9].
* Nice to see that work on getting Linux up and running and fully functional on Apple's proprietary M1 chips is [progressing well][link-20].

## Storage

* Cloudflare recently introduced its own object storage offering, [announced in this blog post][link-2]. Cloudflare's offering, called R2, offers an S3-compatible API and no egress fees, among other features.

## Virtualization

* Eric Sloof has a link to a white paper from VMware on [host power management in vSphere 7.0][link-17].

Although this Tech Short Take is a tad shorter than usual, I still hope that you found something useful in here. Feel free to hit [me up on Twitter][link-99] if you have any feedback. Enjoy!

[link-1]: https://rts.se/networking-and-security-with-nsx-t-in-vsphere-with-tanzu/
[link-2]: https://blog.cloudflare.com/introducing-r2-object-storage/
[link-3]: https://blocksandfiles.com/2021/09/16/cisco-ucs-servers-nowhere-to-go-except-out-of-cisco/
[link-4]: http://www.bladesmadesimple.com/2021/10/cisco-probably-isnt-getting-out-of-the-blade-server-business/
[link-5]: https://www.networkingwithfish.com/understanding-ipv6-7-part-series/
[link-6]: https://blog.apnic.net/2021/09/28/dohot-better-security-privacy-and-integrity-via-load-balanced-dns-over-https-over-tor/
[link-7]: https://www.container-security.site/general_information/container_cve_list.html
[link-8]: https://www.protocol.com/enterprise/ibm-lost-public-cloud
[link-9]: https://www.cyberciti.biz/faq/how-to-install-ffmpeg-with-nvidia-gpu-acceleration-on-linux/
[link-10]: https://aws.amazon.com/blogs/aws/announcing-aws-cloud-control-api/
[link-11]: https://www.pulumi.com/blog/announcing-aws-native/
[link-12]: https://jvns.ca/blog/2021/03/31/dnspeep-tool/
[link-13]: https://rule11.tech/the-rules-of-network-design/
[link-14]: https://aws.amazon.com/blogs/aws/aws-lambda-functions-powered-by-aws-graviton2-processor-run-your-functions-on-arm-and-get-up-to-34-better-price-performance/
[link-15]: https://serverascode.com/2021/08/24/run-privileged-pod-kubectl-run.html
[link-16]: https://rule11.tech/keiths-law-1/
[link-17]: https://www.ntpro.nl/blog/archives/3642-Host-Power-Management-in-VMware-vSphere-7.0-Performance-Study-for-Optimal-Power-Consumption.html
[link-18]: https://blog.styra.com/blog/kong-mesh-and-styra-das-securing-modern-cloud-native-applications
[link-19]: https://containerjournal.com/features/hardening-kubernetes-beyond-nsa-cisa-guidance/
[link-20]: https://asahilinux.org/2021/10/progress-report-september-2021/
[link-21]: https://itnext.io/level-up-your-argo-cd-game-with-applicationset-ccd874977c4c
[link-99]: https://twitter.com/scott_lowe
