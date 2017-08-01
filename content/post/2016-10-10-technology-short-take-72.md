---
author: slowe
categories: Information
comments: true
date: 2016-10-10T00:00:00Z
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- Cumulus
- VMware
- NSX
- Automation
- OpenStack
- Photon
- vSphere
- Terraform
- Consul
- PowerCLI
- Docker
- Microsoft
- Windows
- LXC
- VSAN
- vMotion
- Python
title: 'Technology Short Take #72'
url: /2016/10/10/technology-short-take-72/
---

Welcome to Technology Short Take #72. Normally, I try to publish these on Fridays, but some personal travel prevented that this time around so I'm publishing on a Monday instead. Enough of that, though...bring on the content! As usual, here's my random collection of links, articles, and thoughts about various data center technologies.

## Networking

* Charles Min-Cheng Chan has a write-up on [using IPv6 in Mininet][link-2].
* How's this for Technology Short Take inception---in [TST #70][xref-3], I mentioned the Cumulus topology converter. That prompted Cody Bunch to write this article on [setting up a topology with BGP on Linux using the topology converter][link-3], which I'm now mentioning in this Technology Short Take. (Does your head hurt yet?)
* PowerShell on Cumulus Linux on a network switch? Pete Lumbis [shows you how][link-5]. (There are some very interesting implications around this combination, but I'll reserve that for a future discussion.)
* Someone pointed me toward this article on [using Sophos UTM for remote access to their home lab][link-8].
* Lindsay Hill recently took Simon Wardley's pioneers/settlers/town planners (PST) model---which is itself based on Cringely's _Accidental Empires_ book and the commandos/infantry/police model---and [applied to it the networking industry][link-22]. It's a really good read and helps provide some context to how this model applies to the real world.
* Clifford Haas has [a review of VMware NSX][link-27] (this is from back in January). If you're new to NSX or looking for an overview, this might be helpful.
* Nick Chapman over at CloudFlux shares [his experience working with whitebox switches][link-29], and mulls on why the adoption of whitebox switches in the enterprise may be slower than some might expect (or hope).
* Are you a network engineer who wants to expand his or her automation skills, but doesn't have the equipment? Jason Edelman recently [announced self-service and on-demand network infrastructure][link-32]. It's pretty cool stuff.

## Servers/Hardware

Nothing this time, but I'll keep looking for content to include next time.

## Security

* VMware recently published a case study describing how Rackspace used VMware NSX to quickly meet new PCI DSS compliance goals. Here's [the blog post][link-7] describing the case study, with a link to the actual case study itself.
* Skyport Systems recently said [VMware's Goldilocks project is lost in the woods][link-33]. I've contacted a few folks to try to get more context on the article, but haven't heard back yet. It looks like the post is merely complaining about a lack of progress, though I can't be sure. I'd be quite interested to know if the complaints/concerns are more substantial.

## Cloud Computing/Cloud Management

* John Davidge tackles the OpenStack "Big Tent" and how the OpenStack Foundation [needs to tear down the tent][link-20]. Personally, I agree that OpenStack needs a clearer focus, John's arguments for abolishing the Big Tent seem to make a lot of sense to me.
* Michael Gugino talks about [deploying Nova-LXD hypervisors with OpenStack-Ansible][link-21]. I like the LXD project and it's efforts to make LXC easier to consume, and (to me) including LXD in OpenStack is a natural fit.
* The 1.0 release of Photon Controller is now available on GitHub. This blog post has [a first look at Photon Controller 1.0][link-24].
* Speaking of Photon Controller...if you're trying to figure out when to use Photon Controller and when you should use vSphere Integrated Containers (VIC), then [this post might be helpful][link-25].
* I came across [this article][link-30] that shows Red Hat is allowing Red Hat Virtualization (RHV) to use/leverage OpenStack Neutron networks, thus allowing OpenStack and non-OpenStack workloads to easily communicate with one another. This seems like a really nice approach.

## Operating Systems/Applications

* Raphael Randschau has [an article][link-1] on combining Terraform (an orchestration tool), Nomad (a scheduler), and Consul (a distributed key-value store) to build out infrastructure. If you're not familiar with Terraform or Consul, check out my introductory articles ([here for Terraform][xref-2] and [here for Consul][xref-1]).
* Ivo Beerens [shows you how to fix the error][link-6] related to pairing the broker agent with the Horizon adapter in vRealize Operations.
* [JMESPath][link-10] and `jp` (available [here on GitHub][link-11]) are my new favorite JSON-related topics/tools. I'll probably write up a post about `jp` on its own (as a follow-up of sorts to [my post on `jq`][xref-4]).
* One thing that a lot of folks new to containers (especially Docker containers) seem to struggle with is, "What are some practical starting points?" This post on [using Docker to make using the AWS CLI][link-14] easier is one example (packaging up CLI tools in Docker containers).
* Tim Carr has [a post][link-15] on "hacking Photon OS to do your bidding," but it's really more about setting up an easy-to-use lightweight Linux VM (a useful goal on its own).
* Combine Tim Carr's post (previous bullet) with [this post][link-18] by Robert van den Nieuwendijk on running PowerShell in a Docker container on Photon OS and you've got quite a handy solution.
* Cormac Hogan authored [a post][link-17] that, I think, is quite helpful and timely: a "compare and constrast" between VIC (vSphere Integrated Containers) and Photon Controller. I talked to quite a few customers at VMworld US who weren't clear on the differences between these two solutions. Thanks for the write-up, Cormac!
* The `rkt` container runtime has hit version 1.14.0. [Here's a write-up][link-19] by CoreOS on this version and where `rkt` stands today.
* One of the big news items from this past week was the announcement at MS Ignite of the commercial partnership between Microsoft and Docker. Full details are in [this Docker blog post][link-28], but the gist---as I understand it---is that the commercially-supported variant of Docker Engine will be supplied with Windows Server 2016 and Microsoft will provide support.
* LXD (the container "hypervisor" designed to provide a smooth experience when working with LXC) [recently saw another release][link-31], version 2.4 (followed by a quick 2.4.1 to fix some version strings). This puts LXD at 2.4.1 and LXC at 2.0.4; both products seem to be maturing rather well, though not seeing as much adoption as other alternatives.

## Storage

* I don't really remember _why_ I came across this white paper, but I did, and I thought it might be useful for readers as well: here's [a white paper][link-4] on deploying VSAN over both Layer 2 and Layer 3 network topologies.
* After reading [this article][link-12], I'm wondering: are "fake" VMware Ready Nodes even a thing? Is there really a problem with vendors labeling hardware as VMware Ready and it not actually being certified/validated as VMware Ready?
* Guido Hagemann provides [a very thorough article on VMware ESXi claim rules][link-16].

## Virtualization

* Josh Upton published a recap of [his top 5 take-aways from VMworld 2016][link-9] in Las Vegas. One thing in particular stood out to me from Josh's post: "Other folks want to know how VMware will become the innovation leader again..." This statement makes it clear to me that VMware is not doing a very good job talking about the innovation that occurs within the company. (Either that, or---pursuant to Josh's point #5---it's really just a perception thing that is causing people not to see the innovation.)
* Tom Fenton recently wrote an article on [the evolution of VMware's vMotion technology][link-13]. Two things stuck out to me from the article. First, why use a picture of a dinosaur at the top? Isn't this reinforcing the perception that VMware's technologies are "legacy"? Second, the idea of vMotion to the public cloud sounds nice and all, but there's one little teeny weeny problem: you'd have to solve the problem of how to live migrate workloads across different hypervisors. That being said, if we look at vMotion as a function and not as a specific technology (there's a subtle distinction), then I could potentially see cross-hypervisor live migration as possibility (which would include live migration to the public cloud). It's just going to look very different than the vMotion of today.
* PowerCLI Core, the multi-platform version of PowerCLI, is slated to be released as a Fling soon, according to [this blog post][link-23].
* Work seems to be progressing well on VCA-CLI, a Python-based CLI for working with vCloud Director and vCloud Air. Anthony Spiteri has [more details in his blog post][link-26].

## Career/Soft Skills

I have nothing this time, but I'll stay alert for content to include in the next Short Take.

That's it for TST #72; thanks for reading! I hope you found something useful (or possibly just entertaining, I'll settle for that) along the way.



[link-1]: https://blog.online.net/2016/09/14/build-your-infrastructure-with-terraform-nomad-and-consul-on-scaleway/
[link-2]: http://blog.mcchan.io/ipv6-in-mininet
[link-3]: http://blog.codybunch.com/2016/08/22/Revisiting-BGP-on-Linux-w-Cumulus-Topology-Converter/
[link-4]: http://blogs.vmware.com/virtualblocks/files/2016/03/VSAN-L2_and_L3_Network.pdf
[link-5]: https://community.cumulusnetworks.com/cumulus/topics/powershell-on-cumulus-linux
[link-6]: http://www.ivobeerens.nl/2016/01/13/vrealize-operations-manager/
[link-7]: https://blogs.vmware.com/tam/2016/09/rackspace-leverages-vmware-nsx-case-study.html
[link-8]: http://vmnomad.blogspot.com/2016/09/remote-access-with-sophos-utm.html
[link-9]: http://www.sovsystems.com/vmworld-2016-top-five-list-a-recap/
[link-10]: http://jmespath.org/
[link-11]: https://github.com/jmespath/jp
[link-12]: http://thenicholson.com/buy-fake-ready-nodes/
[link-13]: https://virtualizationreview.com/articles/2016/09/14/evolution-of-vmware-vmotion.aspx
[link-14]: https://lostechies.com/gabrielschenker/2016/09/21/easing-the-use-of-the-aws-cli/
[link-15]: http://www.timcarr.net/?p=471
[link-16]: http://virtualguido.blogspot.com/2016/09/vmware-esxi-claim-rules-unleashed.html
[link-17]: http://blogs.vmware.com/cloudnative/compare-contrast-photon-controller-vs-vic-vsphere-integrated-containers/
[link-18]: https://rvdnieuwendijk.com/2016/08/20/running-powershell-in-a-docker-container-on-vmware-photon-os/
[link-19]: https://coreos.com/blog/rkt-container-engine-v1-14-0.html
[link-20]: https://johndavidge.wordpress.com/2016/09/09/mr-openstack-tear-down-this-tent/
[link-21]: https://medium.com/walmartlabs/deploying-nova-lxd-hypervisors-with-openstack-ansible-39525157879d#.emwa6s8gh
[link-22]: https://lkhill.com/networking-pioneers-settlers-and-town-planners/
[link-23]: http://blogs.vmware.com/PowerCLI/2016/09/powercli_core.html
[link-24]: http://blogs.vmware.com/cloudnative/photon-controller-1-0-first-look/
[link-25]: http://blogs.vmware.com/cloudnative/photon-platform-vsphere-2/
[link-26]: http://anthonyspiteri.net/vca-cli-for-vcloud-director-new-networking-features/
[link-27]: https://www.linkedin.com/pulse/technical-review-vmware-nsx-clifford-haas
[link-28]: https://blog.docker.com/2016/09/docker-microsoft-partnership/
[link-29]: https://cloudflux.co.uk/whitebox-switches-in-the-enterprise/
[link-30]: http://rhelblog.redhat.com/2016/09/19/integrating-red-hat-virtualization-and-red-hat-openstack-platform-with-neutron-networking/
[link-31]: https://linuxcontainers.org/lxd/news/
[link-32]: https://www.linkedin.com/pulse/self-service-demand-network-infrastructure-jason-edelman
[link-33]: https://skyportblog.com/2016/10/04/vmwares-goldilocks-security-lost-in-the-woods/
[xref-1]: {{< relref "2015-02-06-quick-intro-to-consul.md" >}}
[xref-2]: {{< relref "2015-11-25-intro-to-terraform.md" >}}
[xref-3]: {{< relref "2016-08-12-technology-short-take-70.md" >}}
[xref-4]: {{< relref "2015-11-11-handy-cli-tool-json.md" >}}
