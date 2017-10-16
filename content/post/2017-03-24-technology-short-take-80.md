---
author: slowe
categories: Information
comments: true
date: 2017-03-24T00:00:00Z
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- Automation
- Cisco
- NSX
- VMware
- Ansible
- vSphere
- Encryption
- Docker
- OpenStack
- Photon
- Linux
- Vagrant
title: 'Technology Short Take 80'
url: /2017/03/24/technology-short-take-80/
---

Welcome to Technology Short Take #80! This post is a week late (I try to publish these every other Friday), so my apologies for the delay. However, hopefully I've managed to gather together some articles with useful information for you. Enjoy!

## Networking

* Biruk Mekonnen has an introductory article on [using Netmiko for network automation][link-1]. It's short and light on details, but it does provide an example snippet of Python code to illustrate what can be done with Netmiko.
* Gabriele Gerbino has [a nice write-up about Cisco's efforts with APIs][link-4]; his article includes a brief description of YANG data models and a comparison of working with network devices via SSH or via API.
* Giuliano Bertello [shares][link-25] why it's important to RTFM; or, how he fixed an issue with a Cross-vCenter NSX 6.2 installation caused by duplicate NSX Manager UUIDs.
* Andrius Benokraitis provides [a preview of some of the networking features coming soon in Ansible 2.3][link-27]. From my perspective, Ansible has jumped out in front in the race among tools for network automation; I'm seeing more coverage and more interest in using Ansible for network automation.
* Need to locate duplicate MAC addresses in your environment, possibly caused by cloning VMs? Matt Boren has [a PowerShell script over at vNugglets][link-29] that might be helpful.

## Servers/Hardware

Sorry, I don't have anything this time around. Check back next time!

## Security

* Mike Foley tackles the topic of [TLS 1.2 and vSphere][link-14]. 
* Cossack Labs recently released [Acra][link-18], a database encryption suite designed to protect applications against data leaks by providing strong encryption. In this respect, it seems similar to [HexaTier][link-19], a company I spoke with last year (as of this writing, the HexaTier product was no longer available per their website). As opposed to just encrypting data at the transport level (although Acra does that between components of its architecture) or just encrypting data at the storage level (using an encrypted file system or similar), Acra targets encrypting data at the table/row/column level within a database. Acra is open source and available [via GitHub][link-20].
* Federacy recently published an article about the results of their [Docker image vulnerability research][link-22]. There's some good information in this article; I recommend reading it.
* Cisco Systems recently disclosed a vulnerability affecting more than 300 models of switches; see [this Ars Technica article][link-30] for more details. While this may seem like a big deal, it relies on the device being configured for telnet (which is itself something you should have addressed already, IMHO). Thanks to Devender Sharma for the link.

## Cloud Computing/Cloud Management

* Paul Johnston takes a very sensible approach in his post on [cloud first serverless second][link-5]. His point about organizations and individuals needing to focus on the basis for the tools (instead of the tools themselves) is spot on in my opinion, and it underlies the recommendation that organizations/individuals need to become conversant/fluent with cloud operations before trying serverless architectures. (Hat tip to Maish Saidel-Keesing for sharing this article via Twitter.)
* Here's [a post][link-7] outlining some of VMware's more prominent open source projects in the "cloud-native" space. Some of these you've probably heard of, some not.
* Ivan Pepelnjak is best known for his deep networking expertise, but [a recent article of his focused more on OpenStack][link-21]---in particular, some of the "lessons learned" via Paddy Power Betfair's OpenStack Reference Architecture whitepaper. My takeaway? A lot of the design decisions are common sense decisions that you'd see in any other "normal" technology implementation (but are, for whatever reason, often overlooked in attempted OpenStack implementations).
* Gavin Lees shows [how to create a Photon OS container host blueprint][link-24]. Or, in plain English instead of vRA jargon, Gavin shows how to prepare a Photon OS instance that can be deployed by vRA and which registers automatically with Admiral (VMware's open source GUI for deploying containers).

## Operating Systems/Applications

* Massimo Re Ferre takes a stab at [explaining containerd in plain words][link-2].
* GitHub recently changed their Terms of Service (ToS), and this caused some concern about some users. Take a look at [this analysis of the changes][link-3] for some perspective on the changes. (For more concrete analysis if your code/projects are affected, it's best to consult a lawyer.)
* Via Cody Bunch, I found [this GitHub repository][link-6] to help set up development environments on Windows.
* Here are some tips for [advancing in the Bash Shell][link-15].
* Vagrant is a great way to help automate provisioning environments you can use to learn new things. This article on [using Vagrant to turn up an OpenShift Origin cluster][link-16] is a classic example of doing just that.
* [This article][link-17] is a fascinating look into the design considerations for a distributed database; well worth reading in my humble opinion.
* Carlos Nuñez wrote a great article on [why configuration management and provisioning are different][link-23]. I'm so glad to see this article---I said the same thing a while back (in the context of trying to use Ansible to provision/deprovision AWS resources) and took a bit of heat for that comment.

## Storage

* Chris Evans has a post from January talking about [building data storage with containers][link-33] in which he briefly touches upon some storage solutions being built using containers. Maybe I'm missing something, but this doesn't seem as revolutionary to me as it's made out to be---in this context, containers are just another way of delivering the compute functionality necessary to manage the storage and provide data services on top of that storage. Or is there more I'm not seeing?

## Virtualization

* VMware recently [announced the open sourcing][link-8] of some software development kits (SDKs) for the new REST APIs in vSphere 6.5. This is great news, and something VMware needed to do. If you'd like more information on using the SDKs, there's a couple blog posts ([here][link-9] and [here][link-10]) that may be helpful getting you started, and you can also check out [the GitHub landing page][link-11] for the SDKs.
* I've said for a while the whole "containers vs. VMs" situation is a false dichotomy, and apparently I'm not alone in my beliefs. In [this article][link-12], a presentation given by Graham Whaley of Intel discusses the "continuum" that exists between containers and VMs, and how in the future we'll pick and choose the best attributes of both approaches.
* Eric Shanks provides a high-level overview of [creating vSphere images and AWS AMIs using Packer][link-13].
* William Lam [explains a slight change][link-26] in the recently-released vSphere 6.5b patch update, in which users with no vCenter permissions can no longer log into the vSphere Web Client.

## Career/Soft Skills

* Knowing the correct questions to ask and how to ask the questions correctly are important skills. [This article][link-28] helps provide some guidelines. Well worth reading, in my humble opinion.
* Greg Schulz has a two-part series on tradecraft that I found interesting ([part 1][link-31] and [part 2][link-32]). Although he's discussing tradecraft in the context of storage, I think some of the points he makes are applicable to any career (and certainly any IT career).

That's it for this time around. Look for the next Technology Short Take in approximately two weeks. Thanks for reading!



[link-1]: https://www.linkedin.com/pulse/network-automation-part-1-using-netmiko-python-library-mekonnen
[link-2]: https://blogs.vmware.com/cloudnative/docker-containerd-explained-plain-words/
[link-3]: https://www.earth.li/~noodles/blog/2017/03/github-tos-change.html
[link-4]: https://projectme10.wordpress.com/2017/03/06/cisco-wants-you-to-use-apis-and-it-shows/
[link-5]: https://medium.com/@PaulDJohnston/cloud-first-serverless-second-1c086f282326#.7bdzrzdle
[link-6]: https://github.com/accidentaldeveloper/windows-development-environment
[link-7]: https://blogs.vmware.com/cloudnative/enhance-cloud-native-deployments-open-source-projects/
[link-8]: https://blogs.vmware.com/opensource/2017/03/09/integration-vmware-vsphere-using-new-open-sourced-software-development-kits/
[link-9]: https://blogs.vmware.com/code/2017/02/02/getting-started-vsphere-automation-sdk-rest/
[link-10]: https://blogs.vmware.com/code/2017/02/15/getting-started-vsphere-automation-sdk-rest-p2/
[link-11]: https://vmware.github.io/vsphere-automation-sdk/
[link-12]: https://www.linux.com/blog/re-imagining-container-stack-optimize-space-and-speed
[link-13]: http://theithollow.com/2017/03/06/using-packer-create-vsphere-aws-images/
[link-14]: https://blogs.vmware.com/vsphere/2017/03/vsphere-tls-1-2.html
[link-15]: http://samrowe.com/wordpress/advancing-in-the-bash-shell/
[link-16]: https://zwischenzugs.wordpress.com/2017/03/04/a-complete-openshift-cluster-on-vagrant-step-by-step/
[link-17]: https://www.cockroachlabs.com/blog/living-without-atomic-clocks/
[link-18]: https://www.cossacklabs.com/acra/
[link-19]: http://www.hexatier.com/
[link-20]: https://github.com/cossacklabs/acra
[link-21]: http://blog.ipspace.net/2017/03/worth-reading-building-openstack.html
[link-22]: https://www.federacy.com/docker_image_vulnerabilities
[link-23]: https://www.thoughtworks.com/insights/blog/why-configuration-management-and-provisioning-are-different
[link-24]: http://gavoncloud.com/create-photon-os-container-host-blueprint/
[link-25]: http://blog.bertello.org/2017/01/nsx-cross-vc-6-2-control-plane-agent-to-controller-unknown-status-and-nsx-manager-uuid/
[link-26]: http://www.virtuallyghetto.com/2017/03/vsphere-6-5b-prevents-vsphere-web-client-logins-for-users-wo-vc-permissions.html
[link-27]: https://www.ansible.com/blog/networking-features-in-ansible-2-3
[link-28]: http://www.catb.org/esr/faqs/smart-questions.html
[link-29]: http://www.vnugglets.com/2011/08/dev-find-duplicate-vm-nic-mac-addresses.html
[link-30]: https://arstechnica.com/security/2017/03/a-simple-command-allows-the-cia-to-commandeer-318-models-of-cisco-switches/
[link-31]: http://storageioblog.com/data-infrastructure-server-storage-io-related-tradecraft-overview/
[link-32]: http://storageioblog.com/data-infrastructure-server-storage-io-tradecraft-trends/
[link-33]: https://blog.architecting.it/2017/01/13/building-data-storage-with-containers/
