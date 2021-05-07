---
author: slowe
categories: Information
comments: true
date: 2021-05-07T09:15:00-06:00
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- Azure
- Kubernetes
- Go
- AWS
- macOS
- Linux
- JSON
- vSphere
- ActiveDirectory
- Pulumi
- Ansible
title: 'Technology Short Take 140'
url: /2021/05/07/technology-short-take-140/
---

Welcome to Technology Short Take #140! It's hard to believe it's already the start of May 2021---my how time flies! In this Technology Short Take, I've gathered some links for you covering topics like Azure and AWS networking, moving from macOS to Linux (and back again), and more. Let's jump right into the content!<!--more-->

## Networking

* Ivan Pepelnjak takes [a look at why you might want to use Azure Route Server][link-7], followed by [pulling back the covers on how Azure Route Server works][link-8].
* Maegan Jong and Dominik Tornow have a blog series that, in their words, "aims to advance the understanding of Kubernetes and its underlying concepts." Specifically, this post talks about [Kubernetes networking][link-9].
* Michael Kashin combines networking and programming in this post on [getting started with eBPF and Go][link-13].
* [This post on isolated networks on AWS][link-27] takes a pretty comprehensive look at what's required to build isolated AWS networks, including a look at potential data exfiltration paths.

## Servers/Hardware

* Ben Wilcock shares [his experience with an Intel NUC 11][link-1].
* VentureBeat [discusses Armv9][link-5], a new architectural update for Arm-based CPUs.

## Security

* Peyton Smith and Mitchell Moser share [seven common Microsoft Active Directory misconfigurations][link-2] that adversaries tend to abuse.
* Paulos Yibelo describes [exploiting macOS with a text file][link-15].
* The folks at Netskope have a pair of blog posts on GCP OAuth token hijacking in Google Cloud ([part 1][link-16], [part 2][link-17]). These are older posts, from August 2020, and I honestly don't know if the vulnerability still exists (or if it has been patched). If you're a Google Cloud user, this may be worth a closer examination to make sure your accounts are safe.
* Most of [this][link-30] was beyond my comprehension, but I found the tale fascinating to read nevertheless.

## Cloud Computing/Cloud Management

* Stefan Büringer talks about [optimizing Open Policy Agent (OPA)-based Kubernetes authorization][link-3]. Note that this is a slightly older post (about 2 years old), so some of it may no longer apply to the latest versions of OPA and Gatekeeper.
* [This post][link-10] by "xssfox" takes an interesting (to me) look at a  security hole created through the use of an automated code pipeline deploying to a production website.
* I've noted several pundits/experts who have noted the transformational nature of AWS Lambda, and the impact it is having/will have on AWS and its offerings. The [introduction of S3 Object Lambda][link-21] is just the latest example, it seems.
* Chris Evans examines the pricing of virtual instances compared to managed servie offerings as he ponders how hyper-scalers like AWS, Azure, and Google will go about/are going about [optimizing service density][link-22] (i.e., maximizing revenue per hardware instance). It's an interesting observation, for sure (at least, it's interesting to me).
* Marco Lancini discusses [security logging in AWS environments][link-26].
* Pulumi recently released version 3; get more details on the latest release in [this blog post][link-29].

## Operating Systems/Applications

* Justin Garrison [shares some thoughts][link-4] on whiteboarding software (and hardware).
* [Here][link-6] is a reminder why time synchronization remains important.
* Carlos Fenollosa has a series of articles describing his attempt to move to Linux from macOS, and why he came back. Part 3 of the series, found [here][link-14], describes some of the challenges with desktop Linux and why, in his words, "the grass is not greener on the other side."
* Paddy Kelly shows [how to filter JSON data in Ansible using `json_query`][link-18].
* Ivan Pepelnjak's [mention][link-23] of [Network to Code's Schema Enforcer tool][link-24] sent me down the rabbit hole of JSON Schema and validation. Don't be surprised if you see a blog post on this topic pop up soon.
* If you're new to `vim`, [this post][link-28] may be helpful.

## Programming

* Peter Bourgon [speaks out against using build tags for integration tests][link-19].

## Storage

* Cormac Hogan discusses the [VCP-to-vSphere CSI migration process][link-20] (switching from the older in-tree cloud provider to the newer vSphere CSI driver).

## Virtualization

* William Lam [outlines][link-12] some enhancements for USB NIC-only installations that appeared in ESXi 7.0 Update 2.

## Career/Soft Skills

* [There is no recipe for success.][link-11] Well said.
* Former teammate Eric Shanks shares some details on his [home audio/visual setup][link-25].

That's all for now! I hope that I have shared something useful with you. If you have feedback, or if you just want to say hi, feel free to hit [me on Twitter][link-99], or find me on one of the various Slack communities I frequent. Have a great weekend!

[link-1]: https://benwilcock.wordpress.com/2021/02/17/intel-nuc-11-with-pop_os-ubuntu-20-04-lts/
[link-2]: https://www.crowdstrike.com/blog/seven-common-microsoft-ad-misconfigurations-that-adversaries-abuse/
[link-3]: https://itnext.io/optimizing-open-policy-agent-based-kubernetes-authorization-via-go-execution-tracer-7b439bb5dc5b
[link-4]: https://www.justingarrison.com/blog/2021-02-02-remote-whiteboarding/
[link-5]: https://venturebeat.com/2021/03/30/armv9-is-arms-first-major-architectural-update-in-a-decade/
[link-6]: https://blog.ipspace.net/2021/03/terraform-aws-authentication-failure.html
[link-7]: https://blog.ipspace.net/2021/03/azure-route-server-101.html
[link-8]: https://blog.ipspace.net/2021/03/azure-route-server-behind-the-scenes.html
[link-9]: https://dominik-tornow.medium.com/kubernetes-networking-22ea81af44d0
[link-10]: https://sprocketfox.io/xssfox/2021/02/18/pipeline/
[link-11]: https://blog.ipspace.net/2021/03/no-recipe-for-success.html
[link-12]: https://www.virtuallyghetto.com/2021/03/esxi-7-0-update-2-enhancement-for-usb-nic-only-installations.html
[link-13]: https://networkop.co.uk/post/2021-03-ebpf-intro/
[link-14]: https://cfenollosa.com/blog/fed-up-with-the-mac-i-spent-six-months-with-a-linux-laptop-the-grass-is-not-greener-on-the-other-side.html
[link-15]: https://www.paulosyibelo.com/2021/04/this-man-thought-opening-txt-file-is.html
[link-16]: https://www.netskope.com/blog/gcp-oauth-token-hijacking-in-google-cloud-part-1
[link-17]: https://www.netskope.com/blog/gcp-oauth-token-hijacking-in-google-cloud-part-2
[link-18]: https://blog.networktocode.com/post/ansible-filtering-json-query/
[link-19]: https://peter.bourgon.org/blog/2021/04/02/dont-use-build-tags-for-integration-tests.html
[link-20]: https://cormachogan.com/2021/04/14/kubernetes-csi-migration-from-vcp/
[link-21]: https://aws.amazon.com/blogs/aws/introducing-amazon-s3-object-lambda-use-your-code-to-process-data-as-it-is-being-retrieved-from-s3/
[link-22]: https://www.architecting.it/blog/cloud-service-density/
[link-23]: https://blog.ipspace.net/2021/03/schema-enforcer.html
[link-24]: https://github.com/networktocode/schema-enforcer/
[link-25]: https://theithollow.com/2021/04/12/home-audio-visual-setup/
[link-26]: https://www.marcolancini.it/2021/blog-security-logging-cloud-environments-aws/
[link-27]: https://summitroute.com/blog/2020/03/31/isolated_networks_on_aws/
[link-28]: https://jamesdixon.dev/posts/a-minimal-vimrc/
[link-29]: https://www.pulumi.com/blog/pulumi-3-0/
[link-30]: https://blog.polybdenum.com/2021/05/05/how-i-hacked-google-app-engine-anatomy-of-a-java-bytecode-exploit.html
[link-99]: https://twitter.com/scott_lowe
