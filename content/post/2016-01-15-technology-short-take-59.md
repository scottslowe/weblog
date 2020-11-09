---
author: slowe
categories: Information
comments: true
date: 2016-01-15T00:00:00Z
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- Linux
- KVM
- Docker
- NSX
- OpenStack
- SSH
- macOS
- VMware
- vSphere
title: 'Technology Short Take #59'
url: /2016/01/15/technology-short-take-59/
---

Welcome to Technology Short Take #59, the first Technology Short Take of 2016. As we start a new year, here's a collection of links and articles from around the web. Here's hoping you find something useful to you!

## Networking

* Nir Yechiel posted an article on [using the Cumulus VX QCOW2 image with Fedora and KVM][link-6]. Cumulus VX, if you aren't aware, is a community-supported virtual appliance version of Cumulus Linux aimed at helping folks preview and test "full-blown" Cumulus Linux (which, of course, requires compatible hardware).
* NAPALM (Network Automation and Programmability Layer with Multivendor support) looks like a really cool tool. I haven't yet had the opportunity to work with it, but it is definitely something I'd like to explore in more detail. Here's an article on [an effort to add Cisco IOS support to NAPALM][link-16]. Gabriele (the author of that post) also has a nice article on some resources to [get you started with network automation][link-17].
* [Using Python and Netmiko for network automation][link-24] is the topic of this post by Colin McAlister. This is a good introductory post, and one that I plan to leverage as I dive deeper into these tools.
* Kuryr (the OpenStack project to allow Docker Networking to leverage OpenStack Neutron) is coming along. Stuff in this space is moving so quickly at times that it can be difficult to keep up. Fortunately, Gal Sagie is sharing information via his blog; for example, here's [a post on Kuryr support for Docker Networking's pluggable IPAM][link-19] (IP address management).
* Steve Flanders has a good article explaining [how to configure NSX to log to Log Insight][link-27] (a task which, in my humble opinion, is far too complicated and needs to be simplified).

## Servers/Hardware

* [This][link-18] is a _fascinating_ (to me, at least) paper on the implications of non-volatile storage on today's data centers (and data center hardware). It seems clear to me that distributed storage systems are going to be the de facto way to build storage systems moving forward, which obviously has significant implications for networking, compute, power, and environmental factors. Good stuff here---I highly recommend reading this paper.

## Security

* Dwayne Sinclair (an NSX SE at VMware) has [a write-up on what micro-segmentation is _not_][link-13]. Micro-segmentation is one of those terms (like SDN, cloud, DevOps, etc.) that is getting co-opted to mean a lot of different things, and in this post Dwayne talks about why private VLANs aren't actually micro-segmentation.
* You may recall that last year (like that was so long ago!) VMware open-sourced an identity and access management service called Lightwave ([project web site][link-20], [GitHub repo][link-21]). Juan Manuel Rey has taken some time to get Lightwave running in his home lab and has a couple of blog posts that may be worth reading if you're interested in Lightwave. First, he has a post on [setting up a multi-node Lightwave domain][link-22]; once you have a Lightwave domain running, his post on [enabling SSH to authenticate against Lightwave][link-23] may be useful. Good stuff Juan!
* This post has some pretty [in-depth information on the Juniper backdoor][link-29] that was recently uncovered. If I'm understanding it correctly, it was actually a backdoor of an existing vulnerability.
* A moderate security bug in OpenSSH (all releases between 5.4 and 7.1p2) was discovered. I say "moderate" because the impact of the vulnerability is mitigated easily and limited by a number of other factors. [This post by Qualys][link-32] has some great information.

## Cloud Computing/Cloud Management

* Erez Rabih shared [some useful information comparing Kubernetes and ECS][link-7], and why Kubernetes was the right choice for his particular project.
* Here's a handy "how to" on [assigning a specific floating IP address to an OpenStack instance][link-25].
* If you're familiar with AWS, [here's an article][link-31] to help you transition to and/or understand matching Google Cloud concepts.

## Operating Systems/Applications

* Grant Taylor has [a brief article on SSH canonicalization][link-4] and why it might be beneficial to allow SSH to handle canonicalization (instead of allowing the system resolver to do it). (By the way, if you're wondering what _canonicalization_ is, see [this][link-5]. In this case, we are referring to converting a hostname, like `foo`, into a completely unique representation like the fully-qualified domain name [FQDN] `foo.example.com`.)
* Perhaps you feel a little bit like Kev Gorman, and are wondering what in the heck is CNA (cloud-native applications), anyway? If so, have a look at [Kev's post that attempts to help answer that question][link-8].
* Julian Dunn's article on [the oncoming train of enterprise container deployments][link-26] is a good read.
* Early in a new year is a great time for predictions. Here are [some OpenStack and Docker predictions for 2016][link-30].

## Storage

* In case you're interested, here's [a collection of storage trends and predictions for 2016][link-15].

## Virtualization

* Most people are unaware that OS X 10.10 ("Yosemite") introduced some limited hypervisor functionality based on `xhyve` and `Hypervisor.framework`. Maish Saidel-Keesing recently talked about this [in a post about a product named Veertu][link-10], and I also saw this post about [running CoreOS on OS X][link-11]. This virtualization functionality is bare bones in comparison to more full-featured products like VMware Fusion, Parallels, or VirtualBox, but it's still interesting to see how it can be utilized.
* Have I mentioned [the ESXi Virtual Appliance][link-12]?
* William Lam has a walkthrough on [updating the default Photon OS VM template used by AppCatalyst][link-28].

## Career/Soft Skills/Productivity

* A while back, in an earlier Technology Short Take, I mentioned an article on running an effective IRC meeting. The subject of IRC---and its appropriate use---is getting some more attention. First, I read [this article by Chris Dent][link-1] on his viewpoint that persistent IRC connectivity/use is actually _harmful_ to open source communities. (This is a counterpoint to the recommendation for persistent IRC found in [this article][link-2].) I agree with what Chris Dent has to see, and was also pleased to find [this article by Stefano Maffuli][link-3] also calling for a more moderated approach to the use of IRC and a "sane balance of sync-async communication". Well said!
* "DevOps" is one of those terms that I think gets thrown around too much, and so I was thankful to find [this article by Matthew Skelton that lays out some potential team structures][link-9] that address the reality of DevOps as an effort to (Matthew's words) "improve the delivery of value for customers and the business." Many of Matthew's DevOps team topologies could, I think, be equally applied to other IT disciplines as well (who'd like to see the _Smooth Collaboration_ model between the Server and Network teams?).
* If you're thinking about building a home lab, read [this post by Eric Shanks][link-14] first. Home labs are _great_---but there is a cost associated with building and maintaining a home lab, and you'll want to go into this knowing the investment up front.

That's it for this time around. If you have any questions or comments about any of the information included here, feel free to [hit me up on Twitter][link-33]. Thanks for reading!



[link-1]: http://anticdent.org/persistent-irc-considered-harmful.html
[link-2]: https://developer.ibm.com/opentech/2015/12/20/irc-the-secret-to-success-in-open-source/
[link-3]: http://maffulli.net/2015/12/21/balancing-irc-and-email-the-secret-of-success-for-open-collaboration/
[link-4]: http://dotfiles.tnetconsulting.net/articles/2016/0109/ssh-canonicalization.html
[link-5]: https://en.wikipedia.org/wiki/Canonicalization
[link-6]: https://thenetworkway.wordpress.com/2015/12/31/hands-on-with-fedora-kvm-and-cumulus-vx/
[link-7]: https://railsadventures.wordpress.com/2015/12/06/why-we-chose-kubernetes-over-ecs/
[link-8]: http://zerotin.org/2015/11/18/cloud-native-apps-for-the-ops-guy-pt-1-what-the-hell-is-cna-anyway/
[link-9]: http://blog.matthewskelton.net/2013/10/22/what-team-structure-is-right-for-devops-to-flourish/
[link-10]: http://technodrone.blogspot.com/2016/01/native-mac-osx-virtualization-with.html
[link-11]: https://deis.com/blog/2015/get-started-coreos-os-x
[link-12]: http://www.virtuallyghetto.com/2015/12/deploying-nested-esxi-is-even-easier-now-with-the-esxi-virtual-appliance.html
[link-13]: http://www.beyondcli.com/imo/what-micro-segmentation-is-not/
[link-14]: http://theithollow.com/2016/01/04/home-lab-expenses/
[link-15]: https://storpool.com/blog/storage-trends-and-predictions-2016
[link-16]: https://projectme10.wordpress.com/2015/12/07/adding-cisco-ios-support-to-napalm-network-automation-and-programmability-abstraction-layer-with-multivendor-support/
[link-17]: https://projectme10.wordpress.com/2015/12/12/so-you-want-to-start-with-network-automation/
[link-18]: https://queue.acm.org/detail.cfm?id=2874238
[link-19]: http://galsagie.github.io/sdn/openstack/docker/kuryr/neutron/2015/12/30/kuryr-docker-ipam/
[link-20]: https://vmware.github.io/lightwave/
[link-21]: https://github.com/vmware/lightwave
[link-22]: http://blog.jreypo.io/cloud-native/devops/vmware/vmware-lightwave-multi-node-domain-setup/
[link-23]: http://blog.jreypo.io/cloud-native/devops/vmware/enable-ssh-access-against-vmware-lightwave/
[link-24]: http://www.null0.co.uk/2016/01/11/getting-started-with-network-automation-using-python-and-netmiko/
[link-25]: http://trackless.ca/2015/12/21/assigning-a-specific-floating-ip-address-to-an-openstack-instance/
[link-26]: http://www.juliandunn.net/2015/12/04/the-oncoming-train-of-enterprise-container-deployments/
[link-27]: http://sflanders.net/2015/12/21/logging-nsx-with-log-insight/
[link-28]: http://www.virtuallyghetto.com/2015/12/how-to-update-appcatalysts-default-photonos-vm-template-wdocker-1-9.html
[link-29]: http://blog.cryptographyengineering.com/2015/12/on-juniper-backdoor.html
[link-30]: http://natishalom.typepad.com/nati_shaloms_blog/2015/12/6-openstack-docker-predictions-for-2016.html
[link-31]: https://cloud.google.com/docs/google-cloud-platform-for-aws-professionals
[link-32]: https://www.qualys.com/2016/01/14/cve-2016-0777-cve-2016-0778/openssh-cve-2016-0777-cve-2016-0778.txt
[link-33]: https://twitter.com/scott_lowe
