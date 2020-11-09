---
author: slowe
categories: Information
comments: true
date: 2016-12-30T00:00:00Z
tags:
- Networking
- Storage
- Security
- Virtualization
- macOS
- Windows
- Ansible
- Docker
- VMware
- Git
- OVN
- Python
- BSD
- AWS
- Linux
- NSX
- Automation
- OSS
title: 'Technology Short Take #75'
url: /2016/12/30/technology-short-take-75/
---

Welcome to Technology Short Take #75, the final Technology Short Take for 2016. Fortunately, it's not the final Technology Short Take ever, as I'll be back in 2017 with more content. Until then, here's some data center-related articles and links for your enjoyment.

## Networking

* Ajay Chenampara has [some observations][link-2] about running Ansible at scale against network devices.
* Andrey Khomyakov shares some information on automating the setup of whitebox switches running Cumulus Linux in [part 2][link-9] of this series on learning network automation.
* Russell Bryant has shared the results of some testing [comparing ML2+OVS and OVN as backends for OpenStack networking][link-21]. As Russell indicates in his post, some additional analysis is needed to truly understand what's happening, but early looks at the results of his tests show performance improvements in OVN versus ML2+OVS when it comes to total time required to boot a VM.
* Ivan Pepelnjak shares a Python script that creates [Ansible inventory from Vagrant's SSH configuration][link-25]. Handy.

## Servers/Hardware

Nothing this time around!

## Security

* James Green shares [some information on Docker Bench][link-7], a tool designed to help secure Docker environments.
* [Via Mike Foley][link-10], the community now has [a new resource][link-11] to help educate on VM escapes.
* Gordon Turner shares [using OpenBSD 6.0 as a VPN endpoint for iOS and OS X][link-20].

## Cloud Computing/Cloud Management

* While I'm not a fan of the language, I do agree with the sentiment that Drew Firment expresses in [this post][link-8]. Customers _don't_ care about data centers, or DevOps pipelines, or toolkits...they just care about being able to do whatever it is you offer (buy stuff, consume a service, whatever). It's long past time for IT professionals to recognize that IT exists to serve the business, not as an end unto itself.
* Kit Colbert shares [5 predictions][link-13] for cloud-native applications in 2017.
* Ben Thompson has [an in-depth analysis of how Google is challenging AWS][link-14]. The "TL;DR" is that Google's efforts around Kubernetes make it easier for customers to switch to their cloud, and their work around machine learning is the "carrot" that will bring them over. It's a good read; I highly recommend it.
* Kief Morris---author of "Infrastructure as Code" by O'Reilly---has a nice article on the topic of [using pipelines to manage different infrastructure as code environments][link-24].

## Operating Systems/Applications

* Talk about a "blast from the past": way back in 2007, I wrote about a Terminal Services client called TSClientX ([here's the post I wrote][xref-1]). Just in the last couple of weeks, the original author of TSClientX e-mailed me to say that he'd [revived the project][link-1] after 9 years so that users of OS X 10.4 to 10.6 could make RDP connections to modern Windows systems. Way to go, Daniel!
* Michael Preston shares his thoughts on containers in a post titled ["A VMware guy's perspective on containers."][link-4] I know that a lot of my readers are VMware-centric in their jobs and careers, so I think Michael's post may be helpful to a lot of folks. Give it a read, if you get the chance.
* Christian Mohn shares [an approach of storing private information][link-6] in a Git repository along with GitBook for publication/presentation.
* Corban Raun shares some [experiences from using Ansible exclusively for 2 years][link-16]. There's some good stuff in here, especially for newer Ansible users.
* Via Corban's article (see previous bullet), I found [this][link-17]. In a word: Wow.
* Chris Meyers shares some information on [testing Ansible roles using Docker][link-18]. While I agree that using containers as a lightweight isolation mechanism for testing Ansible roles is a good idea, I do have to question whether Docker is the right containerization mechanism. It seems to me that LXD/LXC or systemd-nspawn might be a better approach.
* Alpine Linux 3.5 was [recently released][link-23]. This is notable (for me, at least) primarily because a growing number of Docker images are based on Alpine Linux.
* Bill Ho shares a walkthrough to [setting up vSphere Integrated Containers 0.8.0-rc3][link-26].

## Storage

* Jase McCarty talks about [how to backup or recover storage policy-based management (SPBM) profiles][link-12].

## Virtualization

* Myles Gray shows you [how to recover NSX Manager from a corrupt filesystem][link-3] using an Ubuntu live CD. Neat trick!
* Bill Ho shares the results of some [experimenting with instant clone ESXi on vSAN][link-5] for home lab testing. This is all unsupported stuff, of course, but that hasn't stopped home-labbers before.
* Kyle Ruddy has a post on [getting started with Datacenter CLI (DCLI)][link-15], a new CLI tool that's designed to take advantage of the RESTful APIs introduced with vSphere 6.5. While I'm stoked to see more CLI-centric, scriptable tools being introduced, I do have to wonder why VMware adopted (seemingly) non-standard things like using "+" symbols for command-line options. Why not stick with the standard "-" (for short form) or "--" (for long form) conventions? I hope there's a good technical reason. If not, making it different just for the sake of being different only adds to the "friction" of someone wanting to use it.
* William Lam has some information on [automated vSphere lab deployment][link-19] for vSphere 6.0u2 and vSphere 6.5.
* Roman Dodin [shares a handy Python script][link-27] he wrote called `brvirt` (billed as when "brctl meets virsh"). The script helps manage the relationship between bridge interfaces and VM interfaces. It's [available via GitHub][link-28].

## Career/Soft Skills

* This post by Mark McLoughlin on [sustainable investment in open source][link-22] is a good one for those of us who wish to have a career focused on open source, as it will help us guide our employers (or potential employers) in an industry where open source is increasingly important.

Well, that's it for this time around. It's a bit shorter than usual, but I'm sure you won't mind too much (I'm usually running too long on these posts).

I hope the rest of your 2016 is pleasant, uneventful, and enjoyable! Happy New Year!

[link-1]: http://desktopecho.com/tsclientx/
[link-2]: https://termlen0.github.io/2016/12/16/observations/
[link-3]: https://blah.cloud/virtualisation/recovering-nsx-manager-corrupt-filesystem/
[link-4]: http://blog.mwpreston.net/2016/11/24/a-vmware-guys-perspective-on-containers/
[link-5]: http://billho.website/?p=821
[link-6]: http://vninja.net/misc/using-gitbook-for-secrets/
[link-7]: http://www.actualtech.io/container-hardening-docker-bench-security/
[link-8]: https://cloudrumblings.io/customers-dont-give-a-shit-about-your-devops-pipeline-51a2342cc0f5#.w3jlglsmt
[link-9]: http://packetpushers.net/learning-network-automation-part2/
[link-10]: http://www.yelof.com/2016/12/19/introducing-vmescape-com/
[link-11]: http://vmescape.com/
[link-12]: http://www.jasemccarty.com/blog/spbm-backup-recover-w-powercli/
[link-13]: https://www.vmware.com/radius/five-things-come-cloud-native-applications/
[link-14]: https://stratechery.com/2016/how-google-cloud-platform-is-challenging-aws/
[link-15]: http://blogs.vmware.com/vsphere/2016/12/getting-started-datacenter-cli.html
[link-16]: https://blog.serverdensity.com/what-ive-learnt-from-using-ansible-exclusively-for-2-years/
[link-17]: https://github.com/jlund/streisand
[link-18]: https://www.ansible.com/blog/testing-ansible-roles-with-docker
[link-19]: http://www.virtuallyghetto.com/2016/11/vghetto-automated-vsphere-lab-deployment-for-vsphere-6-0u2-vsphere-6-5.html
[link-20]: http://blog.gordonturner.ca/2016/12/10/openbsd-6-0-vpn-endpoint-for-ios-and-osx
[link-21]: https://blog.russellbryant.net/2016/12/19/comparing-openstack-neutron-ml2ovs-and-ovn-control-plane/
[link-22]: https://crustyblaa.com/sustainable-investment-in-open-source.html
[link-23]: https://alpinelinux.org/posts/Alpine-3.5.0-released.html
[link-24]: https://medium.com/@kief/https-medium-com-kief-using-pipelines-to-manage-environments-with-infrastructure-as-code-b37285a1cbf5#.qqzsylx16
[link-25]: http://automation.ipspace.net/Example:Creating_Ansible_Inventory_from_Vagrant_SSH_Configuration
[link-26]: http://billho.website/?p=801
[link-27]: http://noshut.ru/2016/12/brvirt-when-brctl-meets-virsh/
[link-28]: https://github.com/hellt/brvirt
[xref-1]: {{< relref "2007-02-16-terminal-services-client.md" >}}
