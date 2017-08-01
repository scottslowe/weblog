---
author: slowe
categories: Information
comments: true
date: 2015-02-10T10:33:00Z
tags:
- Networking
- Git
- OVS
- OpenFlow
- Docker
- Security
- Storage
- Virtualization
- OpenStack
- VMware
- vSphere
title: 'Technology Short Take #48'
url: /2015/02/10/technology-short-take-48/
---

Welcome to Technology Short Take #48, another installation in my irregularly-published series that collects links, articles, and thoughts from around the web. This time around, the content is a bit heavier on cloud management and applications/operating systems, but still lots of good content all the way around (I hope, anyway).

## Networking

* Matt Oswalt recently wrapped up his 3-part "DevOps for Networking" series. I referenced part 1 of the series [back in TST #46][xref-1], and parts 2 and 3 are just as good as the first one. Part 2 talks about [source-driven configuration for NetOps][link-7] (which discusses the use of Git and Gerrit to manage network device configurations), while Part 3 walks through a [continuous integration pipeline for networking][link-8] (which adds Jenkins to the mix described in part 2). Helpful and informative content, no question about it.
* The NFV discussion seems to be heating up a bit, particularly the "networking" part of NFV. Craig Matsumoto of SDxCentral recently published [a piece on NFV performance][link-9]; that article was based largely on a blog post by Martin Taylor of Metaswitch found [here][link-10]. The key takeaway is that NFV networking performance requirements are something that projects like OpenStack and Open vSwitch (OVS) haven't yet fully addressed, though progress is being made (particularly on the OVS front, which will soon incorporate Intel's DPDK acceleration for "substantial performance improvements").
* Jason Edelman has posted a self-compiled [list of networking projects that are open source][link-19]; this is a useful list, so thanks for compiling it Jason! I do have one thing to call out, though, and I apologize if I'm showing my ignorance. I didn't think that OpenFlow itself was actually open source (even though there are multiple open source implementations of various OpenFlow products). Can anyone clarify for sure? A quick perusal of the ONF web site does not indicate that the OpenFlow materials are available anywhere under any open source license. (This isn't to say that they aren't freely available, just not available under an open source license.)
* You've probably heard of Socketplane.io, but may not have had the time to take a deeper look at what they are working on. Never fear, this site has [a walkthrough of the Socketplane.io tech preview][link-20] of virtual networks for Docker.
* Tom Hollingworth has a nice article on [getting more bang for the buck with whitebox networking gear][link-22]. Tom's key point is that disaggregating software from hardware---which is kind of a given if you're buying whitebox networking gear---gives you the (potential) flexibility to repurpose network gear based on the software running on it. The "gotcha" is that these software stacks haven't been written yet, so the idea of repurposing hardware from switch to firewall to load balancer is still a bit of a unicorn. However, it's at least _possible_ with whitebox networking gear.

## Servers/Hardware

I have nothing to share this time around, but I'll keep my eyes peeled for stuff to include in the future.

## Security

* Here's a nice article on [a multi-action security workflow][link-23] built using VMware NSX, vShield Endpoint, and vCenter Orchestrator.

## Cloud Computing/Cloud Management

* I came across a couple of useful vCloud Air-related posts that I wanted to share here. First, [here's a workaround][link-3] to the fact that vCA doesn't (yet) do cloud-init, which makes injecting SSH keys into Linux instances a bit difficult. Hats off to Massimo for writing this up. Second, Paco Gómez has [a write-up on running containers on vCloud Air][link-4] (using an Ubuntu instance). Thanks for the walkthrough, Paco!
* Lars Kellogg-Stedman---who really puts out some very useful stuff, and was instrumental in helping me decode some of the Heat-Docker intricacies last year---has [a nice post on the new serial console support in OpenStack Juno][link-6]. Lars also has an outstanding post on [running nova-libvirt and nova-docker on the same host][link-16]; very useful!
* While heavily IBM-focused (perfectly understandable, since he works for IBM), [this post][link-11] by Ronan Dalton shows how to use Docker, Bluemix, and the IBM Containers service together.
* I thought I had referenced this article before, but apparently I haven't. If you're looking for a good treatise on the topic of service discovery, Jason Wilder has written [an excellent article][link-13] that provides a wealth of good information. It's getting a bit dated for this fast-moving space, but the article is a worthwhile read nevertheless (in my opinion).
* Want to run FreeBSD under OpenStack? Check [this out][link-17].
* If you need a greater level of tenant isolation than OpenStack provides by default, Walter Bentley (of Rackspace) has some [information on multi-tenant isolation][link-18] that might be useful.
* Randy Bias recently put up a nice article on [how "vanilla OpenStack" doesn't exist and probably never will][link-27]. I especially liked this quote: "Yes, you should absolutely push to have as much open source in your OpenStack deployment as possible, but since 100% isn’t possible, what [you] should be evaluating is what combination of open source and proprietary get you to the place where you can solve the business problem you are trying to conquer." (correction mine) This is a key point, in my opinion---as IT pros, we exist to help the business succeed. Yes, it's OK to have some principles ("I want as much open source as possible"), but you can't let that ideology get in the way of helping the business succeed.

## Operating Systems/Applications

* Unless you've been living under a rock, no doubt you've heard about [ECS (Amazon EC2 Container Service)][link-2]. In the event you'd like to use CoreOS with ECS, [here are the instructions][link-1] (accomplished easily via cloud-init).
* Although specific to Consul, [this article][link-5] still provides some great information around "config stores" (distributed key/value stores) and using a distributed key/value store for service discovery in Docker environments.
* Cody Bunch has a quick write-up on [using Salt for basic server hardening][link-12]. The post is useful, but---by deliberate choice on Cody's part---it's pretty high-level and basic. It would be great to see a more in-depth treatment of Salt by someone with great writing skills like Cody. Maybe a "Salt for Puppet Users" series?
* The discussion around the "best" way to use Docker containers---as single-process containers or as multi-process containers---continues in a two-part blog series by Phusion. Part 1 describes what is known as [the PID 1 zombie reaping problem][link-14], and part 2 discusses [the use of Baseimage-docker][link-15] as an example base image for Docker containers that provides a solution for the PID 1 zombie reaping problem (among other problems).
* I know that it is heresy to speak out against Docker, but sometimes it really feels like using Docker makes things more complicated than they need to be. This article on [putting data in a volume in a Dockerfile][link-21] is one example.

## Storage

* Jason Boche has [a nice write-up][link-32] on how a QLogic HBA setting prevented NPIV from working in a Dell Compellent environment (used with VMware vSphere). While NPIV remains a "niche" technology that hasn't seen broad adoption, people are still out there using it (or trying), and Jason's post will be helpful in those cases.

## Virtualization

* The big VMware launch event was on February 2, and among the announcements was vSphere 6.0. There are _way_ too many blogs out there to list all of them, so I'll just mention a few here. Eric Shanks has [a "speeds and feeds" post][link-28] on vSphere 6.0, and Julian Wood has [a whole set of posts][link-29] on the new version. So, too, does [Chris Wahl][link-30] (I could've done without the "ZOMG" reference, but different strokes for different folks, right?) as well as [Eric Sloof][link-31]. There are many, _many_ more posts out there, so if I didn't explicitly mention you I'm sorry. Lots of great content happening!

## Career/Soft Skills

I'm adding this section this time around because I have a couple of articles I wanted to share, but I don't know if it will become a regular part of TST or not. (Feedback is welcome; feel free to drop me an e-mail.)

* Iwan Rahabok, a colleague of mine at VMware based in Singapore, recently posted an article on [the rise of the SDDC architect][link-24]. He postulates that, as more and more aspects of the data center fall under virtualization (network virtualization, storage virtualization, etc.) that there is a need for someone to be the SDDC architect, to lead up the broad design across the entirety of the software-defined data center. I don't necessarily disagree, though I do think it might take longer for us to get there than perhaps it should. (Oh, and if you're looking for a good book on vRealize Operations, [look no further][link-25].)
* Via Maish Saidel-Keesing, I saw this article about [comparing your organization's inside with another organization's outsides][link-26]. Great article, and definitely something to keep in mind. The grass isn't always greener on the other side.

It's time to wrap up now; this post is already longer than it probably should be. Here's hoping you found something useful! Although I haven't (yet) enabled comments on the new site, you're always welcome to drop me an e-mail or hit me up on Twitter with any feedback you might have.


[link-1]: https://coreos.com/docs/running-coreos/cloud-providers/ecs/
[link-2]: http://aws.amazon.com/ecs/
[link-3]: http://blogs.vmware.com/vcloud/2014/12/login-vcloud-air-linux-instances-using-ssh-keys.html
[link-4]: http://blog.pacogomez.com/running-containers-on-vcloud-air/
[link-5]: http://progrium.com/blog/2014/08/20/consul-service-discovery-with-docker/
[link-6]: http://blog.oddbit.com/2014/12/22/accessing-the-serial-console-of-your-nova-servers/
[link-7]: http://keepingitclassless.net/2014/11/source-driven-configuration-netops/
[link-8]: http://keepingitclassless.net/2015/01/continuous-integration-pipeline-network/
[link-9]: https://www.sdxcentral.com/articles/news/nfv-performance-bigger-issue/2015/01/
[link-10]: http://info.metaswitch.com/the-switch/tackling-the-nfv-packet-performance-challenge
[link-11]: https://cloudleader.wordpress.com/2015/01/11/docker-bluemix-and-the-ibm-container-service/
[link-12]: http://blog.codybunch.com/posts/2015-01-09-Basic-Server-Hardening-with-Salt/
[link-13]: http://jasonwilder.com/blog/2014/02/04/service-discovery-in-the-cloud/
[link-14]: http://blog.phusion.nl/2015/01/20/docker-and-the-pid-1-zombie-reaping-problem/
[link-15]: http://blog.phusion.nl/2015/01/20/baseimage-docker-fat-containers-treating-containers-vms/
[link-16]: http://blog.oddbit.com/2015/01/17/running-novalibvirt-and-novadocker-on-the-same-host/
[link-17]: http://pellaeon.github.io/bsd-cloudinit/
[link-18]: http://www.hitchnyc.com/openstack-multi-tenant-isolation/
[link-19]: http://www.jedelman.com/home/open-source-networking
[link-20]: http://aucouranton.com/2015/01/16/docker-virtual-networking-with-socketplane-io/
[link-21]: http://jpetazzo.github.io/2015/01/19/dockerfile-and-data-in-volumes/
[link-22]: http://networkingnerd.net/2015/01/27/more-bang-for-your-budget-with-whitebox/
[link-23]: http://www.storagegumbo.com/2014/09/automation-multi-action-security.html
[link-24]: http://virtual-red-dot.info/rise-sddc-architect/
[link-25]: https://www.packtpub.com/virtualization-and-cloud/vmware-vrealize-operations-performance-and-capacity-management
[link-26]: http://watirmelon.com/2015/02/03/never-compare-your-organizations-insides-with-another-organizations-outsides/
[link-27]: http://www.cloudscaling.com/blog/openstack/vanilla-openstack-doesnt-exist-and-never-will/
[link-28]: http://theithollow.com/2015/02/vsphere-6-0-announced/
[link-29]: http://www.wooditwork.com/2015/02/02/whats-new-vsphere-6-0-introduction/
[link-30]: http://wahlnetwork.com/category/deep-dives/vsphere-6-0-zomg/
[link-31]: http://www.ntpro.nl/blog/categories/48-vSphere-6
[link-32]: http://www.boche.net/blog/index.php/2014/12/29/a-common-npiv-problem-with-a-solution/
[xref-1]: {{< relref "2014-11-07-technology-short-take-46.md" >}}