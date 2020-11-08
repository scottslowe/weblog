---
author: slowe
categories: Information
comments: true
date: 2019-08-23T12:00:00Z
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- Apple
- Kubernetes
- Pulumi
- Linux
- vSphere
- VMware
title: 'Technology Short Take 118'
url: /2019/08/23/technology-short-take-118/
---

Welcome to Technology Short Take #118! Next week is [VMworld US][link-0] in San Francisco, CA, and I'll be there live-blogging and meeting up with folks to discuss all things Kubernetes. If you're going to be there, look me up! Otherwise, I leave you with this list of links and articles from around the Internet to keep you busy. Enjoy!<!--more-->

## Networking

* Networking guru Ivan Pepelnjak has migrated his online presence to AWS; read more [here][link-22].

## Servers/Hardware

* Interesting (but otherwise not terribly useful) article on [how to turn a MacBook into a touchscreen][link-12]. Lack of a touch screen remains the MacBook line's second most egregious shortcoming against competing products (the first being the awful keyboard).

## Security

* I came across this article on [tools and methods for auditing Kubernetes RBAC policies][link-10], which had some new tools I hadn't seen before.
* This [web developer security checklist][link-18] has a nice list of some "current recommended practices" for properly securing web applications/sites.
* Daniele Antonioli, Nils Ole Tippenhauer, and Kasper Rasmussen discuss [the KNOB attack][link-19] (a Bluetooth vulnerability).

## Cloud Computing/Cloud Management

* My teammate John Harris has a great post on [ensuring least privilege in Kubernetes using impersonation][link-1]. Good stuff here.
* Alex Brand (also a teammate!) has a post on [using Sonobuoy to test Kubernetes Network Policy enforcement][link-2] (this is to ensure that your installed CNI plugin has the abilitiy to enforce Netowrk Policies).
* I've been using Pulumi quite a bit to help with managing infrastructure environments (spinning up and tearing down test environments). I then found this article by Lee Briggs on [using Pulumi for Kubernetes configuration management][link-15], and it has me interested to continue to explore Pulumi's capabilities. (Oh, and while we're talking about Pulumi, here's another article on [using Pulumi for infrastructure as code][link-17].)
* Gary Stafford has a fairly in-depth article on [managing AWS infrastructure using Ansible, CloudFormation, and CodeBuild][link-24].

## Operating Systems/Applications

* [Here's how][link-4] to create a snippet in VS Code to wrap text with some other text of your choosing. This is handy---I had some similar things set up for Sublime Text, but haven't managed to do the same for VS Code yet.
* Network bonding in Linux---as outlined in [this article][link-5]---is something I may have to explore. Right now I have a custom shell script I wrote that "docks" and "undocks" my laptop by manipulating, among other things, some associated network settings. What I'd _really_ like to have is something like [ControlPlane][link-6] (née Marco Polo for the longtime Mac users), but for Linux. Alas, it appears that even ControlPlane has been abandoned...
* Major Hayden explains [how to use Fedora 30 on GCE][link-7].
* `jq`, `awk`, and `sed`. [Learn them][link-14], use them, love them.
* Ivan Velichko journals his journey of learning about containerization and orchestration in [this blog post][link-20]. If nothing else, this post at least helps clarify the relationship between commonly-mentioned projects like `runC`, `containerD`, `cri-o`, and others.
* Youichi Fujimoto discusses [running Kubernetes locally with KinD and Docker][link-21]. I've found KinD to be easier to use and more flexible than Minikube, but I'm on Linux and have a native Docker installation.
* Tim Little shares some "lessons learned" regarding [increasing resilience in Kubernetes][link-25], such as taking advantage of features like Pod Anti-Affinity, scaling up Deployments to have more than a single Pod, and configuring Readiness probes for all workloads.

## Storage

* Via [this blog post][link-9] by Myles Gray, VMware has announced Cloud Native Storage (CNS), powered by a new CSI (Container Storage Interface) driver for Kubernetes (and other orchestration platforms that use CSI).
* When it comes to storage, Cormac Hogan is an invaluable resource. It's great to see Cormac turning his attention to Kubernetes environments, and in a recent post Cormac tackled [failure scenarios with Kubernetes storage on vSphere][link-16].
* Laurens van Dujin ferrets out a small detail regarding network connectivity while [troubleshooting vSAN Witness Node isolation][link-23].

## Virtualization

* Joey Ketels steps readers through [how to set up RHEL7 desktops with Horizon Instant Clones][link-8].

## Career/Soft Skills

* Via the vExpert Slack instance, I came across this article on [the effect of the scarcity mentality in IT careers][link-3].
* Silvia Botros has a great piece of [how we keep learning][link-11]. There's some good stuff in here, I highly recommend reading it.
* Although written for the creative industry, I think this article by Paul Jun on [five things no one tells you about going full-time again][link-13] probably holds equally true for IT freelancers moving back into corporate life, or even moving from startup culture to big company culture after an acquisition.

I guess that'll have to do for now. It is my sincere hope that you've found something useful in this post, and if you have any feedback I invite you [to contact me on Twitter][link-99]. Thanks for reading!

[link-0]: https://www.vmworld.com/
[link-1]: https://johnharris.io/2019/08/least-privilege-in-kubernetes-using-impersonation/
[link-2]: https://alexbrand.dev/post/testing-kubernetes-network-policy-enforcement-with-sonobuoy/
[link-3]: https://elevaros.com/the-scarcity-mindset-is-sabatoging-your-it-career/
[link-4]: https://www.raymondcamden.com/2019/08/02/creating-a-one-click-visual-studio-code-snippet-to-wrap-content
[link-5]: https://fedoramagazine.org/bond-wifi-and-ethernet-for-easier-networking-mobility/
[link-6]: https://github.com/dustinrue/ControlPlane
[link-7]: https://major.io/2019/08/07/fedora-30-on-google-compute-engine/
[link-8]: https://jketels.nl/how-to-setup-rhel7-with-horizon-instant-clones/
[link-9]: https://blogs.vmware.com/virtualblocks/2019/08/14/introducing-cloud-native-storage-for-vsphere/
[link-10]: https://www.nccgroup.trust/us/about-us/newsroom-and-events/blog/2019/august/tools-and-methods-for-auditing-kubernetes-rbac-policies/
[link-11]: https://blog.dbsmasher.com/2019/08/08/how-we-keep-learning.html
[link-12]: https://www.anishathalye.com/2018/04/03/macbook-touchscreen/
[link-13]: https://99u.adobe.com/articles/53605/5-things-no-one-tells-you-about-going-full-time-again
[link-14]: https://letterstoanewdeveloper.com/2019/07/29/learn-a-little-jq-awk-and-sed/
[link-15]: http://leebriggs.co.uk/blog/2018/09/20/using-pulumi-for-k8s-config-mgmt.html
[link-16]: https://cormachogan.com/2019/06/18/kubernetes-storage-on-vsphere-101-failure-scenarios/
[link-17]: https://www.codegram.com/blog/infrastructure-as-code-with-pulumi/
[link-18]: https://www.sensedeep.com/blog/posts/stories/web-developer-security-checklist.html
[link-19]: https://knobattack.com/
[link-20]: https://iximiuz.com/en/posts/journey-from-containerization-to-orchestration-and-beyond/
[link-21]: https://itnext.io/starting-local-kubernetes-using-kind-and-docker-c6089acfc1c0
[link-22]: https://blog.ipspace.net/2019/08/migrating-ipspacenet-infrastructure-to.html
[link-23]: https://vdr.one/troubleshooting-vsan-witness-node-isolation/
[link-24]: https://itnext.io/managing-aws-infrastructure-as-code-using-ansible-cloudformation-and-codebuild-7edb2e515dff
[link-25]: https://medium.com/kudos-engineering/increasing-resilience-in-kubernetes-b6ddc9fecf80
[link-99]: https://twitter.com/scott_lowe
