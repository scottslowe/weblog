---
author: slowe
categories: Information
comments: true
date: 2017-10-27T12:00:00Z
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- Linux
- OVN
- OVS
- VMware
- AWS
- Ansible
- Python
- Azure
- Microsoft
- HyperV
- Kubernetes
- Fedora
- Docker
title: 'Technology Short Take 89'
url: /2017/10/27/technology-short-take-89/
---

Welcome to Technology Short Take 89! I have a collection of newer materials and some older materials this time around, but hopefully all of them are still useful. (I needed to do some housekeeping on my Instapaper account, which is where I bookmark stuff that frequently lands here.) Enjoy!<!--more-->

## Networking

* This is a slightly older post providing [an overview of container networking][link-8], but still quite relevant. Julia has a very conversational style that works well when explaining new topics to readers, I think.
* Russell Bryant has [a post on Open Virtual Network (OVN)][link-16], a project within the Open vSwitch (OVS) community. If you're not familiar with OVN, this is a good post with which to start.

## Servers/Hardware

Hmm...I didn't find anything again this time around. Perhaps I should remove this section?

## Security

* [This blog post][link-1] discusses some of the new network security functionality available in vSphere Integrated Containers (VIC) version 1.2; specifically, the new _container network firewall_ functionality.
* The NIST and DHS have teamed up on some efforts to secure BGP; more information is available [in this article][link-4].
* When I was using Fedora, I needed some useful information on `firewall-cmd`, and found [this article][link-13] to be helpful.
* Much wailing and gnashing of teeth occurred as a result of [the discovery of the KRACK attack][link-19].

## Cloud Computing/Cloud Management

* Here's a handy tutorial on [using Docker for persisting state across AWS Spot Instances][link-5].
* I like this article on [using Couchbase on AWS from Kubernetes][link-17] because it addresses an often-overlooked (in my opinion) aspect of containerized/microservices architectures: they still need to communicate to external services.
* I wonder how many more Kubernetes provisioning tools will emerge before tool consolidation starts happening? Here's [another one][link-18].
* The 1.8 release of Kubernetes has integration with the 1.0 beta version of containerD (see [this post by Docker][link-20], or visit [the GitHub page for the cri-containerd plugin][link-21]). If you're not familiar with containerD, you may find [this post][link-22] helpful.
* Paul Johnston tackles some myths regarding [vendor lock-in and serverless][link-24].
* Mark Brookfield shares [a bad experience][link-26] he had with running NetBSD on Amazon Web Services. I can certainly see Mark's perspective regarding some perceived failings of AWS; at the same time, I can also understand the need for AWS to limit their support of community-provided AMIs. (At their scale---millions of customers---I can see why they'd need to carefully limit how far they push the support boundary.) For what it's worth, I've never tried NetBSD, but I have yet to run into any similar issues with any distribution of Linux I've tried.

## Operating Systems/Applications

* Here's an example of [using Ansible to setup an environment in OpenBSD][link-2]. That's cool.
* Here's [a Python RegEx (regular expression) to locate MAC addresses][link-3]. That's handy.
* I've talked before about command-line tools to parse JSON (see [here][xref-1]); how about a command-line to create JSON? [Check out `jo`][link-12].
* This [fairly detailed discussion of Azure Functions][link-15] was---for me, anyway---quite useful and helpful.
* Christian Schaller takes [a look back at Fedora Workstation so far][link-23].
* File [this][link-25] under "Interesting But Otherwise Useless". Unless I'm missing something?

## Storage

* Tom Scanlan shares [how to use VIC volumes][link-6] as a way of helping address persistent storage challenges with containers.

## Virtualization

* Want to know more about Hyper-V's inner workings? Via [Ben Armstrong's post][link-9], you might find [the Hypervisor Top-Level Functional Specification][link-10] helpful.
* Most IT professionals---myself included, at times---tend to think only of virtual machines when the term "virtualization" is used. I suppose this is fully expected, given the impact of VMware and hypervisors. However, virtualization is more than that; for example, here's [an example of virtualization applied to database workloads][link-11]. (Note that I haven't tested or seen this product; I saw a reference to it and thought it was interesting.)

## Career/Soft Skills

* Roman Dodin shared [this post][link-7] with me about using GitLab and CloudFlare to host a Hugo-powered blog. I do like the use of GitLab CI to help automate the build of the site; that's pretty handy.
* I can't tell you just how much I agree with this statement from [this post][link-14]: "User groups should not be an avenue for sales." Amen! If you're a partner/vendor/reseller/whatever and you're participating in user group meetings, don't try to turn it into a sales presentation. Make it a _conversation_, an opportunity to build a rapport with customers and potential customers.

That's all for now. Check back again in about 2 weeks for the next Technology Short Take. In the meantime, feel free to [hit me on Twitter][link-27] if you have a link you think I should include in a future post. Thanks for reading!



[link-1]: https://blogs.vmware.com/cloudnative/2017/10/10/solving-container-networking-challenges-vsphere-integrated-containers/
[link-2]: https://github.com/ligurio/openbsd-cookbooks
[link-3]: https://xiix.wordpress.com/2008/06/26/python-regex-for-mac-addresses/
[link-4]: https://www.bleepingcomputer.com/news/technology/new-nist-and-dhs-standards-get-ready-to-tackle-bgp-hijacks/
[link-5]: https://peteris.rocks/blog/persisting-state-between-aws-ec2-spot-instances/
[link-6]: https://blogs.vmware.com/cloudnative/2017/10/24/vsphere-integrated-containers-provide-persistent-storage/
[link-7]: https://netdevops.me/2017/setting-up-a-hugo-blog-with-gitlab-and-cloudflare/
[link-8]: https://jvns.ca/blog/2016/12/22/container-networking/
[link-9]: https://blogs.msdn.microsoft.com/virtual_pc_guy/2017/03/13/new-hypervisor-top-level-functional-specification/
[link-10]: https://docs.microsoft.com/en-us/virtualization/hyper-v-on-windows/reference/tlfs
[link-11]: https://datometry.com/products/database-migration/
[link-12]: http://jpmens.net/2016/03/05/a-shell-command-to-create-json-jo/
[link-13]: https://www.hogarthuk.com/?q=node/9
[link-14]: http://www.42u.ca/2017/06/28/keeping-user-user-groups/
[link-15]: https://cloudacademy.com/blog/microsoft-azure-functions-vs-google-cloud-functions-fight-for-serverless-cloud-domination-continues/
[link-16]: http://next.redhat.com/2017/08/15/understanding-the-open-virtual-network/
[link-17]: https://blog.couchbase.com/using-couchbase-aws-kubernetes-microservices/
[link-18]: https://github.com/jetstack/tarmak
[link-19]: https://arstechnica.com/information-technology/2017/10/severe-flaw-in-wpa2-protocol-leaves-wi-fi-traffic-open-to-eavesdropping/
[link-20]: https://blog.docker.com/2017/09/kubernetes-containerd-integration/
[link-21]: https://github.com/kubernetes-incubator/cri-containerd
[link-22]: https://container42.com/2017/10/14/containerd-deep-dive-intro/
[link-23]: https://blogs.gnome.org/uraeus/2017/10/19/looking-back-at-fedora-workstation-so-far/
[link-24]: https://medium.com/@PaulDJohnston/vendor-lock-in-and-serverless-doesnt-really-exist-cbed790847cb
[link-25]: https://anchore.com/blog/breakdown-of-operating-systems-of-dockerhub/
[link-26]: https://virtualhobbit.com/2017/10/16/getting-the-hell-out-of-amazon-web-services/
[link-27]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2015-11-11-handy-cli-tool-json.md" >}}
