---
author: slowe
categories: Information
comments: true
date: 2020-10-02T14:00:00-07:00
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- DNS
- Kubernetes
- Android
- Cilium
- AWS
- Linux
- Windows
- Microsoft
- Git
- SSH
- vSphere
- VMware
title: 'Technology Short Take 131'
url: /2020/10/02/technology-short-take-131/
---

Welcome to Technology Short Take #131! I'm back with another collection of articles on various data center technologies. This time around the content is a tad heavy on the security side, but I've still managed to pull in articles on networking, cloud computing, applications, and some programming-related content. Here's hoping you find something useful here!<!--more-->

## Networking

* [This recent Ars Technica article][link-3] points out that a feature in Chromium---the open source project leveraged by Chrome and Edge, among others---is having a significant impact on root DNS traffic. More technical details can be found in [an associated APNIC blog post][link-4].
* Here's [a few details][link-11] around Open Service Mesh.
* Quentin Machu outlines a series of problems his company experienced using Weave Net as the CNI for their Kubernetes clusters, as well as describes the migration process to a new CNI. [His blog post][link-18] is well worth a read, IMO.

## Security

* I thought I'd mentioned this already, but there's [a flaw in a Snapdragon chip that puts greater than 1 billion Android phones at risk][link-8], according to Ars Technica.
* Jed Salazar of Isovalent (the company behind Cilium) has [a fairly detailed article][link-12] showing some examples of how Cilium protects against certain network attacks.
* Via [this post by Ivan Pepelnjak][link-15], I found [Nadeem Lughmani's GitHub repository][link-16] aimed to helping to secure one's public cloud deployment.
* Here's [a review of targeted attacks and APTs (advanced persistent threats) on Linux][link-20].
* A _significant_ flaw in Microsoft's Netlogon protocol has been discovered, potentially exposing privilege escalation to Domain Admin. More details are found in [this article][link-25]. The "TL;DR" is you should patch your domain controller "as fast as possible."
* Here's [a quick review of various sandboxing and workload isolation mechanisms][link-29] out there. The article doesn't go deep on any of them, but does provide some useful information and comparison of the various mechanisms.

## Cloud Computing/Cloud Management

* Joaquín Menchaca shares a couple of posts related to EKS---first there's an article on [creating an Amazon VPC for EKS][link-6], followed by an article on [using `eksctl` to create an EKS cluster in said pre-created VPC][link-7].
* Oh, and while we are on the topic of EKS---here's an article by AWS on [using Gatekeeper as a drop-in replacement for Pod Security Policies][link-26], and here's one (also by AWS) on [using Auto Scaling Groups in multiple AZs in Kubernetes][link-27].
* Colin Walters [lays out the arguments against the use of "immutable."][link-10] I get the arguments, but in my humble opinion this is kind of like arguing that there are still servers in serverless.
* Ádám Sándor talks about [some of the bad and the ugly with GitOps][link-19]. This is helpful because many folks only have glowing things to say about GitOps, which---surprise, surprise!---isn't perfect. (Hint: no technology is perfect.)

## Operating Systems/Applications

* I use `scp` all the time. Per [this article][link-9], I guess it's time to switch to `rsync`!
* If you're relatively new to the Linux/macOS/UNIX/*BSD side of the world, this post on [making the most of OpenSSH][link-17] may prove quite useful. Heck, even if you're not new to OpenSSH, you might pick up something new.
* Stepan Stipl writes about [detecting and dealing with deprecated APIs in Kubernetes][link-21].
* Not sure how consensus algorithms work? No worries, start [here][link-24].
* [This][link-28] is an interesting article about why the CockroachDB team created Pebble, a KV store intended to replace RocksDB.
* [Here's a pretty comprehensive resource][link-30] for using `git`.

## Programming

* Kyle Galbraith has two articles that I read over the last several weeks, [one on the repository pattern][link-13] and [one on the adapter pattern][link-14]. Since I'm still quite the programming newbie, both were a bit of a stretch of my knowledge, but I think I gleaned enough of the concepts to be able to use them later.

## Virtualization

* Laurens van Dujin [brings to light][link-5] a bug in vCenter 7.0.0c that causes high CPU usage; turns out this bug is related to the new Workload Control Plane features in vSphere 7. You can disable the service to bring the CPU usage down, but there are caveats. Be sure to read Laurens' post for details.

## Career/Soft Skills

* This is [a great post on some undesirable traits in company culture][link-22]. This post is part 5 in a series; I need to make some time to read some of the earlier posts as well.
* I enjoyed [this post][link-23] by Daniel Teycheney on being proactive and conscious (I might use the word "deliberate") about solving problems in your career.

OK, that's all for now. Hopefully you've found something useful in this post. If so, I'd love to hear about it---feel free to reach out to [me on Twitter][link-99]. Similarly, if you have suggestions for how I might improve the content of these types of posts, I'm open to all constructive criticism. Thanks for reading!

[link-1]: https://aws.amazon.com/blogs/containers/aws-controllers-for-kubernetes-ack/
[link-2]: https://carvel.dev/
[link-3]: https://arstechnica.com/gadgets/2020/08/a-chrome-feature-is-creating-enormous-load-on-global-root-dns-servers/
[link-4]: https://blog.apnic.net/2020/08/21/chromiums-impact-on-root-dns-traffic/
[link-5]: https://vdr.one/vmware-vcenter-7-0-0c-high-cpu-usage/
[link-6]: https://medium.com/@Joachim8675309/create-an-amazon-vpc-for-eks-597481514bcc
[link-7]: https://medium.com/@Joachim8675309/create-eks-with-an-existing-vpc-8e31d95ccc5b
[link-8]: https://arstechnica.com/information-technology/2020/08/snapdragon-chip-flaws-put-1-billion-android-phones-at-risk-of-data-theft/
[link-9]: https://fedoramagazine.org/scp-users-migration-guide-to-rsync/
[link-10]: https://blog.verbum.org/2020/08/22/immutable-%e2%86%92-reprovisionable-anti-hysteresis/
[link-11]: https://openservicemesh.io/blog/introducing-open-service-mesh/
[link-12]: https://cilium.io/blog/2020/06/29/cilium-kubernetes-cni-vulnerability/
[link-13]: https://dev.to/kylegalbraith/getting-familiar-with-the-awesome-repository-pattern--1ao3
[link-14]: https://blog.kylegalbraith.com/2018/06/29/how-to-use-the-excellent-adapter-pattern-and-why-you-should/
[link-15]: https://blog.ipspace.net/2020/09/aws-security-example.html
[link-16]: https://github.com/nadeemnet/NetworkingInPubClouds/tree/master/security
[link-17]: https://linuxconfig.org/how-to-make-the-most-of-openssh
[link-18]: https://blog.quentin-machu.fr/2020/08/07/our-breakup-with-weave-net/
[link-19]: https://blog.container-solutions.com/gitops-the-bad-and-the-ugly
[link-20]: https://securelist.com/an-overview-of-targeted-attacks-and-apts-on-linux/98440/
[link-21]: https://blog.doit-intl.com/kubernetes-how-to-automatically-detect-and-deal-with-deprecated-apis-f9a8fc23444c
[link-22]: https://daverog.com/2020/08/05/culture-eats-technology-for-breakfast/
[link-23]: https://blog.danielteycheney.com/posts/what-problems-do-you-want/
[link-24]: https://www.planetscale.com/blog/blog-series-consensus-algorithms-at-scale-1
[link-25]: https://www.secura.com/blog/zero-logon
[link-26]: https://aws.amazon.com/blogs/containers/using-gatekeeper-as-a-drop-in-pod-security-policy-replacement-in-amazon-eks/
[link-27]: https://aws.amazon.com/blogs/containers/amazon-eks-cluster-multi-zone-auto-scaling-groups/
[link-28]: https://www.cockroachlabs.com/blog/pebble-rocksdb-kv-store/
[link-29]: https://fly.io/blog/sandboxing-and-workload-isolation/
[link-30]: http://www-cs-students.stanford.edu/~blynn/gitmagic/
[link-99]: https://twitter.com/scott_lowe
