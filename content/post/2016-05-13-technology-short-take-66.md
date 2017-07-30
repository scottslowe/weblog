---
author: slowe
categories: Information
comments: true
date: 2016-05-13T00:00:00Z
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- NSX
- VMware
- AWS
- Docker
- OpenStack
- Linux
- Windows
- EMC
- Ansible
title: 'Technology Short Take #66'
url: /2016/05/13/technology-short-take-66/
---

Welcome to Technology Short Take #66! In this post you'll find a collection of links to articles about the major data center technologies. Hopefully something I've included here will be useful to you. Enjoy!

## Networking

* I recently spoke at Interop 2016 in Las Vegas, and while I was there I scribbled down some notes pertaining to how decomposing applications into microservices-based architectures was similar in some respects to decomposing networks into an overlay network and an underlay (physical) network. It's still something I'm exploring, but I hope to get something written up soon. In the meantime, I'd love to hear your thoughts about it. Feel free to [hit me up on Twitter][link-34] or drop me an e-mail.
* While I'm talking about the overlay/underlay model, I found this article by Tom Nolle discussing how [using the overlay/underlay model could enable agile infrastructure][link-33]. It's a good post, well worth reading (in my opinion).

## Servers/Hardware

Nothing this time around. Maybe next time?

## Security

* In the event you're interested in an idea of how much latency the use of in-kernel hypervisor firewalling (such as that offered by VMware NSX) adds, have a look at [this article][link-9] by Sean Howard.

## Cloud Computing/Cloud Management

* Michael Wittig has an article on [using AWS IAM to manage EC2 SSH keys][link-1]. I'll have to give this a try---it's not something I've used before.
* If you think that "infrastructure as code," particularly when in conjunction with cloud platforms (private or public) isn't a thing, then you need only look at all the various orchestration tools designed to help you define infrastructure in some format. You have CloudFormation for AWS (uses JSON), OpenStack Heat (uses JSON or YAML), Terraform (uses JSON or HCL, HashiCorp Configuration Language), and now [Botoform][link-2] (uses YAML). I don't doubt there are more that I've missed (and more that will appear).
* Massimo Re Ferrè recently commented on---in his words---[the "incestuous" relationships][link-6] between all the various orchestration/scheduling tools out there. Is it OpenStack on Kubernetes (aka "Stackanetes"), or Kubernetes on OpenStack (via OpenStack Magnum)? Or Docker on Mesos? Or Mesos on Docker? It almost makes me feel like breaking out in Dr. Seuss' "Fox in Socks"! In any case, Alex Hudson also noticed the trend, and [commented on it][link-7] as well.
* I happened to stumble across [this article][link-10] on setting up an OpenStack Liberty environment using DevStack (along with Nuage Networks for networking). (The author's name isn't clearly listed anywhere on the site, so I can't mention the author by name.)
* Per [RFC 1925][link-18], every old idea will be proposed again with a different name and a different presentation. Is this the case with AWS Lambda, which Massimo thinks is [a return to stored procedures][link-19]?
* Speaking of AWS Lambda, here's [an article by CloudSploit on how they leverage AWS Lambda][link-20] for their security scanning service.

## Operating Systems/Applications

* Nick Janetakis [discusses][link-8] some of the differences it's made for some of his Docker images to switch to using Alpine Linux as the base (instead of the more common Ubuntu or Debian base images).
* The Solinea blog has a post on [deploying Kubernetes with Ansible and Terraform][link-11] that you might find useful/helpful.
* I've heard more than once people describing Docker as a replacement for packaging. Along those lines, Mike English has an article on [distributing command-line tools as Docker containers][link-14].
* So, it's pretty cool that you'll be able to run Windows-based containers (via Docker) on Windows Server 2016; here's [an article that discusses using Packer and Vagrant to try it out yourself][link-15]. What _really_ caught my eye, though, was the size of the Windows Server Docker image (9.429 GB!). I could be wrong, but isn't a 9 GB Docker image kind of the antithesis of what we're trying to achieve with containers? (Stefan Scherer, the author of that post, has a lot of content on Docker and Windows Server, if you're interested.)
* Greg Schulz---who normally tends to focus on storage-related topics---has a post providing [an overview of the Ubuntu 16.04 "Xenial Xerus" release][link-26].

## Storage

* Guido Diepen has [a brief article][link-4] on a script he wrote to clone Docker data volumes. The script itself is [on GitHub][link-5].
* EMC World happened recently in Las Vegas (I was at Interop, also in Las Vegas but at the other end of the Strip), and naturally there were lots of announcements. Probably the easiest thing to do is head over to [Chad Sakac's blog][link-27] and catch up on the 42 new posts coming out of EMC World. (Bear in mind that Chad works for EMC, so his view will be---quite naturally and understandably---a bit skewed. That's not a detrimental statement, just an observation.) For additional coverage, you can also check out [Ed Haletky's summary][link-28], Dave Henry's [commentary and links][link-29], or Matt Healey's [thoughts on Day 3][link-30]. I've no doubt there's much, much more out there, but this should get you started.

## Virtualization

* William Lam wrapped up his multi-part series on test-driving VMware Photon Controller with a post on [deploying Mesos with Photon Controller][link-16] and a post on [deploying Docker Swarm with Photon Controller][link-17].
* Jon Benedict has three posts (so far) on deploying Red Hat Enterprise Virtualization (RHEV) 3.6 ([part 1][link-21], [part 2][link-22], and [part 3][link-23]).
* Alan Renouf has [a post on the PowerCLI module for vRealize Operations][link-25] (vR Ops) that covers the basics of using the module along with some examples. Along the same lines, Luc Dekens is [announcing a PowerShell Log Insight module][link-31] that takes advantage of [Log Insight 3.3's new Query API][link-32].

## Career/Soft Skills

* Read [this article by John Allspaw][link-3]. If you're guilty of using "abstracting" to mean "Not my problem," then you've got a problem.
* I'll put this in this section, though it isn't necessarily the best fit. Jez Humble has [a _great_ article discussing the three key flaws at at the heart of Gartner's "bimodal IT" concept][link-13]; I highly recommend the article. The key takeaway (one of my Twitter followers [called it the "money quote"][link-12]) is that real organizations shouldn't trade off agility for safety, and trading agility for safety is **NOT** what DevOps is about. I won't repeat the entire article here; go read it. Seriously. Go. Now.
* I love idea #5 in this article on helping introverts [find their niche in the office][link-24].

It's time to wrap up now before this post gets any longer. Here's hoping you found something useful here!



[link-1]: https://dzone.com/articles/manage-aws-ec2-ssh-access-with-iam
[link-2]: https://github.com/russellballestrini/botoform
[link-3]: http://www.kitchensoap.com/2016/04/28/abstract-as-a-verb/
[link-4]: https://www.guidodiepen.nl/2016/05/cloning-docker-data-volumes/
[link-5]: https://github.com/gdiepen/docker-convenience-scripts
[link-6]: http://www.it20.info/2016/03/the-incestuous-relations-among-containers-orchestration-tools/
[link-7]: http://www.alexhudson.com/2016/04/29/containing-incestuousness/
[link-8]: http://nickjanetakis.com/blog/alpine-based-docker-images-make-a-difference-in-real-world-apps
[link-9]: http://nsxperts.com/?p=100
[link-10]: https://pinrojas.com/2016/04/03/building-a-nuageopenstack-demo-at-home-part1/
[link-11]: http://solinea.com/blog/deploying-kubernetes-ansible-terraform
[link-12]: https://twitter.com/andyhky/status/729720208009535489
[link-13]: http://continuousdelivery.com/2016/04/the-flaw-at-the-heart-of-bimodal-it/
[link-14]: https://spin.atomicobject.com/2015/11/30/command-line-tools-docker/
[link-15]: https://stefanscherer.github.io/setup-local-windows-2016-tp5-docker-vm/
[link-16]: http://www.virtuallyghetto.com/2016/04/test-driving-vmware-photon-controller-part-3b-deploying-mesos.html
[link-17]: http://www.virtuallyghetto.com/2016/04/test-driving-vmware-photon-controller-part-3c-deploying-docker-swarm.html
[link-18]: https://tools.ietf.org/html/rfc1925
[link-19]: http://www.it20.info/2016/04/aws-lambda-a-few-years-of-advancement-and-we-are-back-to-stored-procedures/
[link-20]: http://blog.cloudsploit.com/2015/09/15/how-we-use-lambda/
[link-21]: http://captainkvm.com/2016/05/deploying-rhev-pt1/
[link-22]: http://captainkvm.com/2016/05/deploying-rhev-3-6-pt2/
[link-23]: http://captainkvm.com/2016/05/deploying-rhev-3-6-pt3-storage/
[link-24]: http://workawesome.com/office-life/6-ways-introverts-find-niche-in-office/
[link-25]: http://blogs.vmware.com/PowerCLI/2016/05/getting-started-with-powercli-for-vrealize-operations-vr-ops.html
[link-26]: http://storageioblog.com/ubuntu-16-04-lts-aka-xenial-xerus-whats-in-the-bits-and-bytes/
[link-27]: http://virtualgeek.typepad.com/virtual_geek/
[link-28]: https://www.virtualizationpractice.com/emcworld-announcements-37503/
[link-29]: http://geekfluent.com/2014/05/05/summary-of-emcs-monday-announcements/
[link-30]: http://www.neuralytix.com/?p=8033
[link-31]: http://www.lucd.info/2016/04/29/loginsight-module/
[link-32]: http://sflanders.net/2016/04/20/log-insight-3-3-query-api/
[link-33]: http://blog.cimicorp.com/?p=2629
[link-34]: https://twitter.com/scott_lowe