---
author: slowe
categories: Information
comments: true
date: 2019-08-02T12:00:00Z
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- NSX
- Automation
- Docker
- AWS
- Kubernetes
- Linux
- YAML
- Ansible
title: 'Technology Short Take 117'
url: /2019/08/02/technology-short-take-117/
---

Welcome to Technology Short Take #117! Here's my latest gathering of links and articles from the around the World Wide Web (an "old school" reference for you right there). I've got a little bit of something for most everyone, except for the storage nerds (I'm leaving that to my friend J Metz this time around). Here's hoping you find something useful!<!--more-->

## Networking

* Ronald de Jong [sheds some light][link-4] on a potential "gotcha" with distributed multi-tier routing configuration in NSX. The key takeaway here is that anytime there is firewalling or other stateful services in place, the traffic will route to the (non-distributed) service router instead of just the distributed routers.
* Northbound Networks provides [some information on the P4 programming language][link-9].
* Via the Packet Pushers blog, Daniel Himes provides [a guide to a starting point for enterprise (network) automation][link-11].

## Servers/Hardware

* AnandTech has a nice article on [the future PCIe 6.0 spec][link-24], expected in to land in 2021.

## Security

* Ronnie Flathers [shares how he uses Docker for penetration testing][link-12].
* Interested in running Hashcat on AWS EC2? See [this article][link-15] by Ben Mason.
* Another data breach, this time from Capital One, hit the news this past week. Corey Quinn has his take on the breach [here][link-26], and Brian Krebs has more information [here][link-27].

## Cloud Computing/Cloud Management

* Dan Finneran has a great introductory post on [first steps with `govmomi`][link-1].
* Iman Tumorang shares [how to use a private instance of Google Container Registry (GCR) from a Kubernetes cluster][link-6].
* Mohamed Labouardy has a write-up showing how to pull together various Google services, including GKE, Cloud Build, and GCR along with Terraform and Packer, [to build a CI/CD workflow][link-19].
* [This][link-23] is probably the strongest use case I've seen so far for using a general-purpose programming language for infrastructure-as-code. The linked article uses TypeScript with Pulumi, but the concept---being able to extend existing concepts via a general purpose programming language to add new functionality---applies equally well to other environments as well.

## Operating Systems/Applications

* Steve Sloka explains [how to run Contour (a Kubernetes ingress controller) using `kind`][link-2].
* Nicholas Lane digs into [using Kubernetes' `client-go` dynamic client to work with namespaced Custom Resource Definitions (CRDs)][link-3].
* David Holder talks briefly about [application security with mutual TLS (mTLS) via Istio][link-5]. I take exception to the use of "application security" in David's title, which I (personally) take to mean something more than mutual TLS (which is more about application identity and authentication/authorization). However, that's a minor nit, and the article is a good introduction to what mTLS is and how it may be implementing using Istio.
* Fatih Arslan shares [some tips for writing idempotent Bash scripts][link-7].
* Daniel Weibel has [ideas for boosting your `kubectl` productivity][link-10].
* I recently stumbled across [`kube-score`][link-13], which performs static analysis of Kubernetes object definitions (for more information, see [the GitHub repository][link-14]).
* The Kubernetes ecosystem is rapidly exploring ideas to help with the creation and manipulation of YAML (and JSON) for object definitions. Two new (to me, at least) options are CUE and `jk`. Gareth Rushgrove describes CUE in [this post][link-16] (also see [the CUE website][link-17] for more information), and Damien Lespiau [briefs readers on the `jk` tool][link-18] for generating object definitions using TypeScript.
* Some of the Kubernetes APIs are being deprecated in the upcoming 1.16 release; more details are available [here][link-20].
* Ian Miell talks about [purging Docker from all his home servers][link-21].

## Storage

Nothing from me this time around, but if you're really hurting for some storage news and views, check out [Storage Short Take #12][link-25] from J Metz.

## Virtualization

* Gavin Stephens talks about [using Ansible to automate VM and OVA appliance deployments][link-8].

## Career/Soft Skills

* James Beswick [explores][link-22] the cloud skill shortage and why it exists even though there are plenty of folks who are certified. The "TL;DR" is, in my mind, captured in this statement: "They want people with solid experience, a risk-free bet that the new hire can execute the tech flawlessly." I'd say this is a strong indicator that experienced IT folks who are able to assimilate new skills are going to have an advantage moving forward.

OK, enough is enough, time to wrap it up. As always, feel free to engage with [me on Twitter][link-99] if you have any questions, corrections, suggestions, or other feedback. Thanks!

[link-1]: https://thebsdbox.co.uk/2019/07/04/First-steps-with-Govmomi/
[link-2]: https://projectcontour.io/kindly-running-contour/
[link-3]: https://soggy.space/namespaced-crds-dynamic-client/
[link-4]: https://my-sddc.net/2019/07/17/distributed-multi-tier-routing-in-nsx-t/
[link-5]: https://www.virtualthoughts.co.uk/2019/07/15/application-security-with-mutual-tls-mtls-via-istio/
[link-6]: https://medium.com/hackernoon/today-i-learned-pull-docker-image-from-gcr-google-container-registry-in-any-non-gcp-kubernetes-5f8298f28969
[link-7]: https://arslan.io/2019/07/03/how-to-write-idempotent-bash-scripts/
[link-8]: https://www.simplygeek.co.uk/2019/07/18/automate-vsphere-virtual-machine-and-ova-appliance-deployments-using-ansible/
[link-9]: https://northboundnetworks.com/blogs/sdn/what-is-the-p4-programming-language
[link-10]: https://learnk8s.io/blog/kubectl-productivity/
[link-11]: https://packetpushers.net/a-guide-to-a-starting-point-for-enterprise-automation/
[link-12]: https://blog.ropnop.com/docker-for-pentesters/
[link-13]: https://kube-score.com/
[link-14]: https://github.com/zegl/kube-score
[link-15]: https://ben.the-collective.net/2019/07/10/hashcat-in-aws-ec2/
[link-16]: https://garethr.dev/2019/04/configuring-kubernetes-with-cue/
[link-17]: https://cue.googlesource.com/cue
[link-18]: https://damien.lespiau.name/posts/2019-06-12-jk-configuration-as-code/
[link-19]: https://medium.com/foxintelligence-inside/building-a-ci-cd-on-gcp-with-kubernetes-db8455d7286e
[link-20]: https://kubernetes.io/blog/2019/07/18/api-deprecations-in-1-16/
[link-21]: https://zwischenzugs.com/2019/07/27/goodbye-docker-purging-is-such-sweet-sorrow/
[link-22]: https://itnext.io/the-cloud-skills-shortage-and-the-unemployed-army-of-the-certified-bd405784cef1
[link-23]: https://blog.d2si.io/2019/07/22/aws-tagging-strategy-pulumi/
[link-24]: https://www.anandtech.com/show/14559/pci-express-bandwidth-to-be-doubled-again-pcie-60-announced-spec-to-land-in-2021
[link-25]: https://jmetz.com/2019/07/storage-short-take-12/
[link-26]: https://www.lastweekinaws.com/blog/capitalones-capitaltwo-day/
[link-27]: https://krebsonsecurity.com/2019/07/capital-one-data-theft-impacts-106m-people/
[link-99]: https://twitter.com/scott_lowe
