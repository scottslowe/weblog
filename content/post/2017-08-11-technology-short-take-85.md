---
author: slowe
categories: Information
comments: true
date: 2017-08-11T09:00:00Z
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- NSX
- PowerShell
- AWS
- Ansible
- Automation
- Intel
- Kubernetes
- Microsoft
- Windows
- Docker
- Linux
- NVMe
- VMware
- vSphere
- Vagrant
- Writing
- Fusion
- VirtualBox
title: 'Technology Short Take 85'
url: /2017/08/11/technology-short-take-85/
---

Welcome to Technology Short Take #85! This is my irregularly-published collection of links and articles from around the Internet related to the major data center technologies: networking, hardware, security, cloud computing, applications/OSes, storage, and virtualization. Plus, just for fun, I usually try to include a couple career-related links as well. Enjoy!<!--more-->

## Networking

* Want to install VMware NSX in your home lab, but concerned about resource utilization? Here's a guide to [deploying NSX in home labs with limited resources][link-13]. (Although it's not called out in the article, it's important to note that this is not supported by VMware.)
* Jeffrey Kusters has a write-up on [how to integrate AWS and vRealize Network Insight 3.4][link-14]. This allows you to visualize network activity on AWS using vRNI (as on-premises traffic, assuming you've configured vRNI appropriately).
* Gareth Lewis walks through [a vCNS 5.5.4 to VMware NSX 6.2.5 upgrade][link-15].
* It's a bit Ansible-skewed (as expected given it's posted on the Ansible site), but networking pros looking for some perspectives on using Ansible for network automation might find [this post][link-26] useful.
* [PowerNSX in a container?][link-27] Sure!
* I know that I saw this article from Matt Oswalt before, but for some reason when I came across it again while putting this post together it just _really_ resonated with me. Networking professionals, it's time to realize [your cheese moved a long time ago][link-28].

## Servers/Hardware

* There's a lot of talk about how Nvidia is "owning" the AI movement, but as Derrick Harris points out, [Intel still owns the data center][link-19].
* Here's [a good "what you need to know" article][link-25] by Kevin Houston on the new Intel Xeon Scalable Processor (SP), recently announced.

## Security

* [This post][link-12] was a particularly interesting (to me, at least) examination of another aspect of IoT security; specifically, the ZigBee protocol.

## Cloud Computing/Cloud Management

* Soenke Ruempler has a good overview of [why an AWS multi-account architecture is probably better][link-6] once you start working in AWS at any real (beyond lab testing) scale. This is, in my opinion, a really good article that draws upon a number of different sources to make recommendations. Well worth reading.
* This post by Ross Kukulinski describes [shell autocompletion for `kubectl`][link-9]. Handy!
* I found [this post][link-10] on how Atlassian designed their Kubernetes infrastructure on AWS to be helpful, if for no other reason than as a real-life example of how an organization reconciles AWS design considerations with Kubernetes design considerations.

## Operating Systems/Applications

* Chances are you've heard of CRI, the Container Runtime Interface for Kubernetes (which is a way of standardizing how Kubernetes interacts with an underlying container runtime). CRI-O is the effort to implement CRI for OCI-compatible runtimes like `runC`. [This post][link-1] helps explain CRI-O in a bit more detail.
* Looks like Microsoft's Nano Server is [headed in a new direction][link-3].
* Ajeet Singh Raina has [an overview of some new networking functionality][link-7] available in the RC5 release of Docker 17.06. Raina specifically calls out MACVLAN driver support in Swarm mode (recall that I discussed MACVLAN [here][xref-1] about 18 months ago) as an example of the new networking functionality, and proceeds to provide some examples on how to use MACVLAN networking with Docker on Google Cloud Platform. I found this last part interesting, because early testing on my part with MACVLAN on AWS didn't work; I guess this is one of those differences between GCP and AWS.
* Feeling geeky? Read [this article][link-8] on diving deep into Windows to figure out what was causing an erratic performance issue despite plenty of hardware resources.
* If you're interested in learning more about BPF/eBPF, check out these two posts ([here][link-17] and [here][link-18]) from Julia Evans. (I'm of the opinion that eBPF portends a major change in Linux networking, and given the prevalance of Linux in networking portends a change in the networking industry as a whole.)
* I came across this article thanks to The New Stack on Twitter (good account to follow for "cloud-native" sorts of things): how software development [tends to creep back towards the monolith][link-29]. It's a good read, and a good reminder that tools alone can't replace discipline and rigor when it comes to building microservices-based architectures.

## Storage

* J Metz offered to send me a few links for storage, so I happily took him up on that offer. First up we have [the announcement of the release of the NVMe 1.3 specification][link-4], which adds a number of new features and expands the suitability of the specification to more markets (like mobile devices). Next up is [a brief review][link-5] of M&A (merger and acquisition) activity in the storage industry in the first half of 2017. There are some well-known entries there (such as Nimble's acquisition by HPE), but also some not-so-well-known ones (such as Double-Take's acquisition by Carbonite). Thanks for the links, J! (By the way, all readers are more than welcome to send over links they feel might be useful to other readers. Don't just send me press releases, please.)
* WekaIO recently came out of stealth, touting their "cloud-scale storage software." It looks interesting; check out [the press release here][link-20].

## Virtualization

* Jason Brooks outlines how to get [up and running with oVirt 4.1 and Gluster][link-2].
* William Lam tackles a[n issue deploying vSphere Integrated Containers 1.1][link-21] (due to a lack of SHA256 support in PowerCLI). (See [this related article][link-22] as well.)
* Jason Boche [describes the workaround][link-23] to an obscure bug involving VMware Horizon shared folders with Windows 10.
* John Slack shares some tips and tricks garnered while [learning to use Vagrant on Windows 10][link-24]. Since I may end up with a Windows 10 box in my home office setup soon (a long story I'll share elsewhere when appropriate), some of this information may prove quite useful to me. I'm sure others will find it useful as well.
* Chris Bradshaw has a PowerCLI script he wrote to [test ESXi host network connectivity][link-30] (useful for verifying new hosts are connected correctly).
* I'm looking forward to the changes that are making their way into VMware Fusion; see [this note on the VMware Fusion 2017 Technical Preview][link-31]. As many of you know, I [switched from Fusion to VirtualBox][xref-2] some time ago, and I'm hopeful that this release will address my concerns so I can switch back.

## Career/Soft Skills

* Frank Denneman's recent release of _VMware vSphere 6.5 Host Resources Deep Dive_ led him to write about [the core motivation for writing a book][link-11]. As a fellow author, I can wholeheartedly endorse and support Frank's statements in this article. If you're thinking about writing a book, definitely read this post. As Frank says, I'm not saying don't do it---I'm just advocating for being well-educated before you jump in.
* I found Matt Klein's article on [why he will not start an Envoy platform company][link-16] interesting, and I think there are lessons there that many of us can apply to our own career decisions/directions.

I think that's enough for this time around; here's hoping you found something useful and pertinent. As I mentioned above in the Storage section, if you have some links you've found useful that you'd like to share with me, I'd love to see them (and maybe they'll make their way into a future Tech Short Take!). Thanks for reading, and have a great weekend!



[link-1]: https://www.projectatomic.io/blog/2017/06/6-reasons-why-cri-o-is-the-best-runtime-for-kubernetes/
[link-2]: https://www.ovirt.org/blog/2017/04/up-and-running-with-ovirt-4.1-and-gluster-storage/
[link-3]: http://www.zdnet.com/article/microsofts-nano-server-what-to-expect-this-fall/
[link-4]: http://www.businesswire.com/news/home/20170621005405/en/NVMe-Revision-1.3-Expands-Reach-Fast-Storage
[link-5]: http://www.storagenewsletter.com/2017/06/21/14-mas-in-storage-industry-at-mid-year-2017/
[link-6]: https://ruempler.eu/2017/07/09/advantages-aws-multi-account-architecture/index.html
[link-7]: http://collabnix.com/docker-17-06-swarm-mode-now-with-macvlan-support/
[link-8]: https://randomascii.wordpress.com/2017/07/09/24-core-cpu-and-i-cant-move-my-mouse/
[link-9]: https://blog.heptio.com/kubectl-shell-autocomplete-heptioprotip-48dd023e0bf3
[link-10]: https://developer.atlassian.com/blog/2017/07/kubernetes-infra-on-aws/
[link-11]: http://frankdenneman.nl/2017/07/11/exploring-core-motivation-writing-book/
[link-12]: https://blog.acolyer.org/2017/06/22/iot-goes-nuclear-creating-a-zigbee-chain-reaction/
[link-13]: http://www.virten.net/2016/05/deploy-vmware-nsx-in-homelabs-with-limited-resources/
[link-14]: https://www.jeffreykusters.nl/2017/07/04/integrate-aws-vrealize-network-insight-vrni-3-4/
[link-15]: http://www.virtualaspirations.com/2017/02/20/vmware-vcns-5-5-4-to-nsx-6-2-5-upgrade/
[link-16]: https://medium.com/@mattklein123/optimizing-impact-why-i-will-not-start-an-envoy-platform-company-8904286658cb
[link-17]: https://jvns.ca/blog/2017/06/28/notes-on-bpf---ebpf/
[link-18]: https://jvns.ca/blog/2017/04/07/xdp-bpf-tutorial/
[link-19]: https://architecht.io/dont-forget-that-intel-still-owns-the-data-center-473944d15ce2
[link-20]: http://www.marketwired.com/press-release/wekaio-eliminates-need-legacy-external-storage-with-cloud-scale-storage-software-2225816.htm
[link-21]: http://www.virtuallyghetto.com/2017/06/workaround-to-deploy-vsphere-integrated-containers-1-1-ova-using-powercli-sha256-not-supported.html
[link-22]: http://www.virtuallyghetto.com/2016/11/default-hashing-algorithm-changed-in-ovftool-4-2-preventing-ovfova-import-using-vsphere-c-client.html
[link-23]: http://www.boche.net/blog/index.php/2017/06/12/vmware-horizon-share-folders-issue-with-windows-10/
[link-24]: https://blogs.technet.microsoft.com/virtualization/2017/07/06/vagrant-and-hyper-v-tips-and-tricks/
[link-25]: http://bladesmadesimple.com/2017/07/what-you-need-to-know-about-intel-xeon-sp-cpus/
[link-26]: https://www.ansible.com/blog/five-questions-network-automation
[link-27]: http://networkinferno.net/contain-yourself-powernsx-please
[link-28]: https://keepingitclassless.net/2017/04/cheese-moved-long-time-ago/
[link-29]: http://shiroyasha.io/monorepos-monoliths-in-disguise.html
[link-30]: http://isjw.uk/test-esxi-network/
[link-31]: https://blogs.vmware.com/teamfusion/2017/07/tech-preview-2017.html
[xref-1]: {{< relref "2016-01-28-docker-macvlan-interfaces.md" >}}
[xref-2]: {{< relref "2016-09-28-why-now-using-virtualbox-with-vagrant.md" >}}
