---
author: slowe
categories: Information
comments: true
date: 2023-09-08T09:00:00-06:00
tags:
- Azure
- Cilium
- Cumulus
- Docker
- Encryption
- Hardware
- IaC
- iOS
- Kubernetes
- Linux
- macOS
- Microsoft
- Networking
- Pulumi
- Security
- Solaris
- Storage
- Sun
- Terraform
- Velero
- Virtualization
- VMware
- Wireless
title: 'Technology Short Take 170'
url: /2023/09/08/technology-short-take-170/
---

Welcome to Technology Short Take #170! I had originally intended to get this published before the long Labor Day weekend, but didn't quite have it ready. So, here you go---here's your latest collection of links from around the internet focused on data center and cloud-related technologies. I hope that you find something useful here.<!--more-->

## Networking

* Brent Salisbury [shares some code][link-19] intended to help you "ChatGPT your project documentation." What does that mean, exactly? It's about indexing your project documentation so as to expose it---and the information within it---via a user-friendly natural language model.
* This is a slightly older article (from January of this year) in which Josh Saul of Hedgehog [pays homage][link-21] to the achievements of Cumulus Networks in the open networking movement.
* Michael Kashin has a good post on [source IP address selection in Linux][link-24].
* Amit Gupta shows [how to install Cilium in Azure Kubernetes Service (AKS) without `kube-proxy`][link-25].
* Here are some [design considerations for Wi-Fi 6E (6GHz) networks][link-26].

## Servers/Hardware

* I must admit that I always wanted to have a Sun workstation, and I've had an interest in SunOS/Solaris for years (check out [this link](/tags/solaris/) if you don't believe me). So, it's natural that this post on [reliving the glory days of Sun workstations][link-27] would catch my attention.

## Security

* Michael Tsai [weighs in][link-8] on the Microsoft signing key that was stolen, sharing several links with commentary on this matter.
* Exploiting cloud VMs via a remote serial/console service? Yikes. Fortunately, [this Microsoft Security Response Center article][link-9] not only shows how to use Azure Serial Console to compromise sensitive information, but also shows how to detect exploitation activity. What about preventing it?
* As detailed in [this article][link-15], it turns out BitLocker can be bypassed---assuming physical access to the hardware---with a cheap logic analyzer.
* Daniel Stenberg rails about [everything that is wrong about CVEs][link-16].
* Grafana recently [had to rotate their GPG signing key][link-22].
* Time to update your iOS, iPadOS, and macOS devices! A new zero-click, zero-day exploit was announced and Apple has released an update for all affected systems. More details on the exploit are available [here][link-28].

## Cloud Computing/Cloud Management

* I've long been a fan of Velero, the application formerly known as Heptio Ark. It was, therefore, very satisfying to see not one but _two_ Velero-related posts emerge recently from the VMware blogging community. First, Stellios Williams talks about [using Velero to back up and restore things on TKGm][link-1]. Next, Bassem Rezkalla takes readers through [backing up and restoring TKGS guest clusters using Velero][link-2] (somewhat related to my own discussion of the topic [here][xref-1], but not focused on TKGS).
* This is a slightly older post (dates from 2021), but still useful, I think. In it, Christopher Lenard discusses [the benefits of moving from Terraform to Pulumi][link-3].
* André Ilhicas shows [how to run a sidecar container with Docker Compose][link-4]. I must admit I was not aware of the `network_mode` setting that makes this possible. Good to know!
* Warren Parad discusses [metrics, alerts, and determining what's important][link-6].
* My colleague Lee Briggs (brilliant dude, by the way) wrote this article on [structuring your infrastructure as code][link-7].
* Here's [another take][link-14] on the future of Terraform following the license switch. And, while we are talking about "another take," here's [another take on OpenTF][link-17] (the proposed "truly open source" Terraform fork).
* Soham Dutta shares [a handy trick][link-23] to improve AWS EC2 security with a strong metadata block.

## Operating Systems/Applications

* I started being a fan of Basic Apple Guy a while ago, and I use some of his wallpapers on my Mac and my mobile devices. (I'm weird/obsessive like this, but I like using matching wallpapers across all my devices.) Anyway, he released a couple a while ago that I'm just getting around to sharing here: [a revamped version of his revamped macOS Tiger][link-11], and [a "parody" wallpaper for OS X Rancho Cucamonga][link-12] (there's a story there). What's that---which wallpaper of his am I currently using? I'm currently using the Twilight variation of macOS Tiger Redux.
* Howard Oakley [talks][link-20] about App Translocation (formerly known as Gatekeeper Path Randomization) in macOS. While I generally enjoy using macOS, sometimes the tight control that Cupertino exercises over the OS and its behaviors feels...constricting.
* Jeff Geerling [walks][link-29] readers through testing the Coral TPU accelerator using Docker (in order to work around some Python library dependency issues).

## Programming/Development

* For what is perhaps an alternative viewpoint on the role of AI coding assistants, check out Rizèl Scarlett's post on [learning `p5.js` with GitHub Copilot][link-5]. (Disclaimer: It's my understanding that Rizèl works for GitHub as a developer advocate, so keep that in mind when reading the post.)
* Troy Hunt provides some detail on how he [fights API bots using Turnstile from Cloudflare][link-10]. It's a pretty interesting read; this is a Cloudflare feature I wasn't really aware of.

## Virtualization

* William Lam shares a quick tip for [preserving some values when using VMware vCenter Converter][link-13] (namely, the MachineGUID and the BIOS UUID). I didn't think vCenter Converter saw much use anymore!
* Mateusz Romaniuk writes about [upgrading an ESXi host from version 7 to version 8 using vLCM][link-18].

That's all for now! I'm always open to reader feedback, so if you have feeback for me feel free to contact me. My e-mail address is not terribly hard to find, and you can also use either [Twitter][link-99], [Mastodon][link-30], or [Bluesky][link-31] to contact me. I also tend to lurk in a number of Slack communities, so you're welcome to contact me there as well. I'd love to hear from you!

[link-1]: https://www.funkycloudmedina.com/2023/08/backups-and-restores-using-velero-in-tkgm-1.6.1/
[link-2]: https://nsxbaas.blog/2023/08/17/backup-restore-tkgs-guest-clusters-using-standalone-velero/
[link-3]: https://medium.com/@christopherlenard/the-benefits-of-moving-from-terraform-to-pulumi-7e01a3ab8f43
[link-4]: https://ilhicas.com/2023/01/26/How-to-run-a-sidecar-in-docker-compose.html?expand_article=1
[link-5]: https://dev.to/github/how-im-using-github-copilot-to-learn-p5js-39gh
[link-6]: https://dev.to/authress/aws-metrics-advanced-40f8
[link-7]: https://leebriggs.co.uk/blog/2023/08/17/structuring-iac
[link-8]: https://mjtsai.com/blog/2023/08/23/microsoft-signing-key-stolen-by-chinese/
[link-9]: https://msrc.microsoft.com/blog/2023/08/azure-serial-console-attack-and-defense-part-1/
[link-10]: https://www.troyhunt.com/fighting-api-bots-with-cloudflares-invisible-turnstile/
[link-11]: https://basicappleguy.com/basicappleblog/macos-tiger-second-edition
[link-12]: https://basicappleguy.com/basicappleblog/os-x-rancho-cucamonga
[link-13]: https://williamlam.com/2023/05/quick-tip-preserving-machineguid-in-windows-using-vcenter-converter.html
[link-14]: https://matduggan.com/terraform-is-dead-long-live-pulumi/
[link-15]: https://www.errno.fr/BypassingBitlocker.html
[link-16]: https://daniel.haxx.se/blog/2023/08/26/cve-2020-19909-is-everything-that-is-wrong-with-cves/
[link-17]: https://medium.com/@hello_9187/why-we-are-not-supporting-opentf-a46855f52dc4
[link-18]: https://vmattroman.com/how-to-upgrade-esxi-host-from-version-7-to-version-8-with-vlcm/
[link-19]: http://networkstatic.net/chatgpt-project-docs/
[link-20]: https://eclecticlight.co/2023/05/09/what-causes-app-translocation/
[link-21]: https://githedgehog.com/thank-you-cumulus/
[link-22]: https://grafana.com/blog/2023/08/24/grafana-security-update-gpg-signing-key-rotation/
[link-23]: https://mr-right.medium.com/locked-and-loaded-fortifying-aws-ec2-security-with-an-ironclad-metadata-block-d0366dcd8315
[link-24]: https://networkop.co.uk/post/2023-09-linux-src/
[link-25]: https://medium.com/@amitmavgupta/installing-cilium-in-azure-kubernetes-service-byocni-with-no-kube-proxy-825b9007b24b
[link-26]: https://wi-fight-it.blogspot.com/2023/08/wi-fi-6e-6ghz-design-considerations.html
[link-27]: https://hackaday.com/2023/04/15/relive-the-glory-days-of-sun-workstations/
[link-28]: https://citizenlab.ca/2023/09/blastpass-nso-group-iphone-zero-click-zero-day-exploit-captured-in-the-wild/
[link-29]: https://www.jeffgeerling.com/blog/2023/testing-coral-tpu-accelerator-m2-or-pcie-docker
[link-30]: https://fosstodon.org/@scottslowe
[link-31]: https://bsky.app/profile/scottslowe.bsky.social
[link-99]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2021-01-11-using-velero-to-protect-cluster-api.md" >}}
