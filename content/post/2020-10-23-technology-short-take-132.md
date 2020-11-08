---
author: slowe
categories: Information
comments: true
date: 2020-10-23T15:00:00-07:00
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- Kubernetes
- Linux
- Microsoft
- Apple
- macOS
- Docker
- AWS
- VMware
- Go
- JSON
title: 'Technology Short Take 132'
url: /2020/10/23/technology-short-take-132/
---

Welcome to Technology Short Take #132! My list of links and articles from around the web seems to be a bit heavy on security-related topics this time. Still, there's a decent collection of networking, cloud computing, and virtualization articles as well as a smattering of other topics for you to peruse. I hope you find something useful!<!--more-->

## Networking

* I think a fair number of folks may not be aware that the Nginx ingress controller for Kubernetes---both the community version and the Nginx-maintained open source version---do suffer from timeouts and errors resulting from changes in the back-end application's list of endpoints (think pods being added or removed). [This performance testing post][link-5] lays out all the details. In particular, see the section titled "Timeout and Error Results for the Dynamic Deployment."
* Ivan Pepelnjak [attempts to answer][link-7] the question, "How much do I need to know about Linux networking?"
* Speaking of Linux networking...Marek Majkowski of Cloudflare [digs deep into conntrack][link-9], used for stateful firewalling functionality.

## Servers/Hardware

* Normally I talk about server hardware and such here, but with so much moving to public cloud providers, let's expand that focus a little bit: in [this post][link-16], Jeramiah Dooley provides his perspective on the Surface Duo after a month of use.

## Security

* I recently stumbled across [this utility][link-11] to help protect your macOS-based system against persistent malware.
* I'm not sure if I should put this under "Hardware" or here under "Security": Apple's T2 chip has [an "unfixable vulnerability"][link-13] that could lead to significant system compromise. There's more detail available in [this post][link-17] as well.
* [Here's][link-15] an interesting read: the story of some security researchers who hacked on Apple for three months.
* Brad Geesaman has [a write-up on CVE-2020-15157][link-18], aka "ContainerDrip," that you may want to review.
* Intel has released [a security advisory for BlueZ][link-19], which is related to Bluetooth support in the Linux kernel.
* It appears that Apple may have left themselves a "network backdoor" in macOS Big Sur. [This article][link-21] provides links to a Twitter thread that outlines the backdoor in more detail, but the gist of the situation is that kernel extensions have been deprecated in Big Sur and their replacement _appears_ not to affect some Apple applications (most notably the App Store).

## Cloud Computing/Cloud Management

* Brandon Willmott has a post outlining [the important directories to know][link-3] when working with Kubernetes (it's also helpful for the CKA exam).
* Docker recently open-sourced the Docker Compose integration for Amazon ECS and Microsoft ACI. This code hasn't made it into the `docker-compose` CLI yet. [This Docker blog post][link-6] has more details.
* This is a slightly older post, but Rich Burroughs has [a nice summary/recap of KubeCon EU 2020][link-8].
* Ahmed Bham and Marcelo Boeira of AWS have a walkthrough for [migrating a self-managed Kubernetes cluster on EC2 to Amazon EKS][link-27].
* Yann Hamon of Contentful shares that they have [open-sourced a Kubernetes operator][link-28] to sync Kubernetes Secrets from AWS Secrets Manager.
* In [this post][link-30], Docker shares they they are delaying the enforcement of their new image retention policy, and reminds folks of the image pull rate limits that are due to start on November 1. I know that Docker Hub must consume enormous resources for the company (and thus has a large associated cost), but limiting the ubiquity of Docker Hub---and thus driving developers/users elsewhere---seems shortsighted. I guess time will tell.

## Operating Systems/Applications

* Chip Zoller has information on [deploying Harbor on Photon OS][link-12].
* Here's [a decent article on using `tee` and `xargs`][link-14].
* [This is a neat trick][link-25] to enhance Git's `diff` functionality.
* Maarten Van Driessen [shows readers][link-26] that clearing the DNS cache on PhotonOS is just a matter of restarting the DNSMasq service.

## Storage

* Duncan Epping [walks readers through VMware Cloud Disaster Recovery][link-4], which---if I'm reading this correctly---is the evolution of the Datrium product.

## Programming

* Alex Edwards has compiled [a list of "surprises" and "gotchas"][link-20] that come from working with Go's `encoding/json` package.

## Virtualization

* Patrick Kremer shares some information on [using the VMC (VMware Cloud on AWS) API to troubleshoot the connected VPC][link-1].
* Robert Guske [takes readers through a guide][link-22] aimed at setting up a local VEBA environment using `kind` and the vCenter simulator. VEBA, for those who aren't familiar, is the [VMware Event Broker Appliance][link-23].
* Anthony Spiteri talks about [deploying Tanzu on a single ESXi host][link-24].

## Career/Soft Skills

* Ben Kuhn shares some information on [how to create more immersive video calls][link-10].
* In a post written in the context of network engineers learning automation tools, Ethan Banks shares that [you don't need to become a developer but simply use their tools][link-29]. I think that this maxim holds true for other disciplines as well, not just network engineers.

That's all for now, folks! Thanks for taking the time to read, and I hope that I was able to share something you'll find useful. If you have any feedback on this post, or on the site in general, feel free to hit [me on Twitter][link-99]. I'd love to hear your feedback!

[link-1]: http://www.patrickkremer.com/using-the-vmc-api-to-troubleshoot-the-connected-vpc/
[link-2]: https://www.learncloudnative.com/blog/2020-09-26-init-containers/
[link-3]: https://brandonwillmott.com/2020/10/01/important-directories-to-know-for-kubernetes-cka-exam/
[link-4]: http://www.yellow-bricks.com/2020/09/30/announcing-vmware-cloud-disaster-recovery/
[link-5]: https://www.nginx.com/blog/performance-testing-nginx-ingress-controllers-dynamic-kubernetes-cloud-environment/
[link-6]: https://www.docker.com/blog/open-source-cloud-compose/
[link-7]: https://blog.ipspace.net/2020/09/grasping-linux-networking.html
[link-8]: https://firehydrant.io/blog/kubecon-2020-europe-wrapup/
[link-9]: https://blog.cloudflare.com/conntrack-tales-one-thousand-and-one-flows/
[link-10]: https://www.benkuhn.net/vc/
[link-11]: https://objective-see.com/products/blockblock.html
[link-12]: https://neonmirrors.net/post/2020-10/deploying-harbor-on-photon-os/
[link-13]: https://appleinsider.com/articles/20/10/05/apples-mac-t2-chip-has-an-unfixable-vulnerability-that-could-allow-root-access
[link-14]: https://www.tecmint.com/run-commands-from-standard-input-using-tee-and-xargs-in-linux/
[link-15]: https://samcurry.net/hacking-apple/
[link-16]: https://jeramiah.net/2020/10/a-month-with-the-surface-duo/
[link-17]: https://ironpeak.be/blog/crouching-t2-hidden-danger/
[link-18]: https://darkbit.io/blog/cve-2020-15157-containerdrip
[link-19]: https://www.intel.com/content/www/us/en/security-center/advisory/intel-sa-00435.html
[link-20]: https://www.alexedwards.net/blog/json-surprises-and-gotchas
[link-21]: https://appleterm.com/2020/10/20/macos-big-sur-firewalls-and-vpns/
[link-22]: https://rguske.github.io/post/spin-up-a-local-veba-environment-with-kind-and-vcenter-simulator/
[link-23]: https://vmweventbroker.io/
[link-24]: https://anthonyspiteri.net/deploying-vmware-tanzu-with-ha-proxy/
[link-25]: https://tekin.co.uk/2020/10/better-git-diff-output-for-ruby-python-elixir-and-more
[link-26]: https://www.brisk-it.net/clear-dns-cache-vcsa-photonos/
[link-27]: https://aws.amazon.com/blogs/architecture/field-notes-migrating-a-self-managed-kubernetes-cluster-on-ec2-to-amazon-eks/
[link-28]: https://www.contentful.com/blog/2020/10/20/open-source-kube-secret-syncer/
[link-29]: https://packetpushers.net/dont-become-a-developer-but-use-their-tools/
[link-30]: https://www.docker.com/blog/docker-hub-image-retention-policy-delayed-and-subscription-updates/
[link-99]: https://twitter.com/scott_lowe
