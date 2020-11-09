---
author: slowe
categories: Information
comments: true
date: 2017-02-03T00:00:00Z
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- NSX
- VMware
- OVN
- OpenStack
- Apple
- macOS
- vSphere
- Linux
- CLI
- Microsoft
- SSH
- Docker
- VirtualBox
- SSL
title: 'Technology Short Take #77'
url: /2017/02/03/technology-short-take-77/
---

Welcome to Technology Short Take #77. I've got a new collection of links and articles from around the Web on various data center-focused technologies.

## Networking

* Mark Brookfield [takes a moment][link-6] to remind everyone that you shouldn't use the (deprecated) C# vSphere Client to manage NSX environments. Good advice.
* Michael Kashin has [a great article][link-9] on how Open Virtual Network (OVN, part of the Open vSwitch project) implements virtual networks in OpenStack. Yes, the article is slightly OpenStack-centric, but it still remains a very informative look at the different components of OVN and how OVN works. (You might also be interested in [an earlier article][link-10] that outlines how to build and install OVN with OpenStack.)
* In a bit of an older post from late summer 2016, Matt Oswalt outlines [why network engineers should care about the network software supply chain][link-11]. I won't steal Matt's thunder; go have a look at the post yourself to see if you agree with his assessment.
* Simon Leinen (from SWITCHengines) explains their use of [IPv6 with OpenStack][link-15].
* John Kozej has a write-up on an [NSX logical switch packet walk][link-17].
* Sam McGeown discusses [deploying ECMP with NSX for a provider logical router][link-18].
* Thanks to Ivan Pepelnjak, I saw [this network diagnostic tool][link-29].

## Servers/Hardware

* There was a fair amount of wailing and gnashing of teeth when Apple updated the MacBook Pro line with the Touch Bar. Some people love it, others absolutely hate it. Jeff Geerling has a great article on [why he returned his 2016 MacBook Pro with Touch Bar][link-33]; it's definitely worth a read, in my opinion.

## Security

* Vivek Gite over at nixCraft explains [how to use ufw (Uncomplicated Firewall) on Ubuntu to limit SSH connections][link-19].
* If you're interested in learning more about some of the new security features in vSphere 6.5, check out [this post][link-20] by Mike Foley---he has pointers to more details on VM Encryption, Secure Boot, and Encrypted vMotion.
* Eddie Cranklin Kim shares [how to create a virus using assembly language][link-28]. (I'm assuming Eddie isn't actually advocating the creation of viruses, but using this as a means of teaching assembly language programming techniques.)
* Lennart Poettering, the creator of systemd, shares [how to avoid CVE-2016-8655 using systemd][link-30].
* Check out [this list of command-line basics][link-32] that Robert Graham feels every cybersecurity professional should know/learn.

## Cloud Computing/Cloud Management

* If you've deployed the vRealize Operations Management Pack for NSX, there's an option to enable Log Insight integration as well. When this option is checked, NSX will be configured to log to Log Insight as described in [this post by Steve Flanders][link-4]. Normally, that's not an issue, but be aware that this prevents you from changing the log destinations as they'll just be changed right back shortly afterward.
* Sayli Karmarkar and Vinay Shah of Netflix [describe Winston][link-12], "an event-driven diagnostic and remediation platform." What does that mean? Basically, in a nutshell, Winston executes runbooks of automation code in response to events. Have a look at the article for more details.
* Steve Schofield shares [how to change the Docker default network to persist across reboots][link-21] with vRealize 7.2.

## Operating Systems/Applications

* Ben Corrie shares [some of his thoughts][link-3] about the recent GA of vSphere Integrated Containers (VIC).
* [Flatpak][link-5] is a (relatively) new application packaging/sandboxing mechanism for Linux applications. This looks really promising, IMHO---I'm excited to see it continue to develop.
* Nicolas Malaval has a post describing [how to record SSH sessions established through a bastion host][link-8]. The post is a bit geeky but quite informative, and worth reading if SSH bastion hosts are a key part of your architecture. (Not sure what a bastion host is? Read [this post][xref-1].) Thanks to Maish Saidel-Keesing for pointing out this article.
* Who would have thought that one day you'd refer to a Microsoft web site for instructions on configuring something in Linux? That's exactly what we have here: [a Microsoft Azure page with instructions on configuring DHCPv6 for Linux VMs][link-13] (covering various Linux distributions).
* This next post is more than a year old; it's been sitting in my "folder of articles that I'm going to discuss but haven't gotten around to yet". In any case, I think the time is right. [The article][link-14] is by Kelsey Hightower, and in it he discusses how building twelve-factor apps incorrectly can lead to "12 fractured apps" (the title of the article). As I understand it, the basic idea behind Kelsey's article is that _if_ you're going to go down the route of containerizing your applications, _then_ you should do it right instead of jimmy-rigging shell scripts and configuration management tools.
* Courtesy of Cody Bunch, I found this article on [defensive BASH programming][link-16], which contains some very useful tips of which I was not aware.
* Looks like I'm not the only one making the leap from macOS to Linux---check out this pair of articles on Wesley Moore's switch ([part 1][link-26] and [part 2][link-27]). Part 2 is especially helpful for others who might be switching, as it contains a list of Linux apps to replace the macOS equivalents.

## Storage

* A new, Docker-specific filesystem and graph driver have emerged to address the shortcomings of existing implementations. The new filesystem is called LCFS, or Layer Cloning File System, and you can get more details on LCFS [via its GitHub repository][link-34].

## Virtualization

* This post outlines [how to install the VirtualBox Guest Additions][link-1] on recent versions of Fedora/CentOS/RHEL.
* William Lam shares his adventure(s) in testing [a USB-C/Thunderbolt 3 adapter for ESXi][link-2].
* Adam Eckerle (along with some help from Mike Foley) [discusses the VMCA and a "hybrid" approach to managing SSL certificates][link-7] in a vSphere 6.x environment.
* Eric Gray published this article on [creating an Auto Deploy reverse proxy cache using Nginx][link-22].
* In this post, Massimo Re Ferre talks about a script he wrote to [automate vSphere Integrated Containers][link-23] (the script itself is [on GitHub][link-24]).

## Career/Soft Skills

* John Cook shares a very important aspect of using automation: it's not necessarily about saving time, but also about [saving mental energy][link-25].
* I often recommend that IT professionals try to improve their understanding of the business side of things. This article on [the failure of RethinkDB][link-31] is a good read that helps, in my opinion, shed some light on the non-technical aspects of a technology business.

That's all this time around. Here's hoping you found something useful!



[link-1]: https://www.if-not-true-then-false.com/2010/install-virtualbox-guest-additions-on-fedora-centos-red-hat-rhel/
[link-2]: http://www.virtuallyghetto.com/2017/01/functional-usb-c-thunderbolt-3-ethernet-adapter-for-esxi-5-5-6-0-6-5.html
[link-3]: http://bensdoings.com/2017/01/19/vics-uncool-ga/
[link-4]: http://sflanders.net/2015/12/21/logging-nsx-with-log-insight/
[link-5]: http://flatpak.org/
[link-6]: https://virtualhobbit.com/2017/01/27/dont-cripple-your-nsx-installation-with-host-profiles-and-the-c-client/
[link-7]: https://blogs.vmware.com/vsphere/2017/01/walkthrough-hybrid-ssl-certificate-replacement.html
[link-8]: https://aws.amazon.com/blogs/security/how-to-record-ssh-sessions-established-through-a-bastion-host/
[link-9]: http://networkop.co.uk/blog/2016/12/10/ovn-part2/
[link-10]: http://networkop.co.uk/blog/2016/11/27/ovn-part1/
[link-11]: https://keepingitclassless.net/2016/08/importance-network-supply-chain/
[link-12]: http://techblog.netflix.com/2016/08/introducing-winston-event-driven.html
[link-13]: https://docs.microsoft.com/en-us/azure/load-balancer/load-balancer-ipv6-for-linux
[link-14]: https://medium.com/@kelseyhightower/12-fractured-apps-1080c73d481c#.v4n8th7ac
[link-15]: https://cloudblog.switch.ch/2016/07/07/switchengines-under-the-hood-basic-ipv6-configuration/
[link-16]: http://www.kfirlavi.com/blog/2012/11/14/defensive-bash-programming/
[link-17]: https://thewificable.com/2017/01/30/nsx-logical-switch-packet-walk/
[link-18]: http://www.definit.co.uk/2017/01/deploying-ecmp-for-nsx-provider-logical-router/
[link-19]: https://www.cyberciti.biz/faq/howto-limiting-ssh-connections-with-ufw-on-ubuntu-debian/
[link-20]: https://blogs.vmware.com/vsphere/2017/01/vsphere-6-5-security-product-walkthroughs.html
[link-21]: http://iislogs.com/steveschofield/2017/01/12/change-docker-default-network-to-persist-reboots-and-vrealize-automation/
[link-22]: http://www.vcritical.com/2017/01/easy-auto-deploy-reverse-proxy-cache-with-an-nginx-container/
[link-23]: http://www.it20.info/2017/01/automating-vsphere-integrated-containers-deployments/
[link-24]: https://github.com/mreferre/vic-product-machine
[link-25]: https://www.johndcook.com/blog/2015/12/22/automate-to-save-mental-energy-not-time/
[link-26]: http://bitcannon.net/post/finding-an-alternative-to-mac-os-x/
[link-27]: http://bitcannon.net/post/finding-an-alternative-to-mac-os-x-part-2/
[link-28]: https://cranklin.wordpress.com/2016/12/26/how-to-create-a-virus-using-the-assembly-language/
[link-29]: https://github.com/mehrdadrad/mylg
[link-30]: http://0pointer.net/blog/avoiding-cve-2016-8655-with-systemd.html
[link-31]: http://www.defstartup.org/2017/01/18/why-rethinkdb-failed.html
[link-32]: http://blog.erratasec.com/2017/01/the-command-line-for-cybersec.html
[link-33]: https://www.jeffgeerling.com/blog/2017/i-returned-my-2016-macbook-pro-touch-bar
[link-34]: https://github.com/portworx/lcfs
[xref-1]: {{< relref "2015-11-21-using-ssh-bastion-host.md" >}}
