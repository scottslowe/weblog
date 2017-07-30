---
author: slowe
categories: Information
comments: true
date: 2016-07-22T00:00:00Z
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- Python
- Docker
- Cumulus
- MPLS
- VMware
- NSX
- AWS
- OpenStack
- Kubernetes
- Photon
- Ansible
- DNS
- vSphere
- vMotion
title: 'Technology Short Take #69'
url: /2016/07/22/technology-short-take-69/
---

Welcome to Technology Short Take #69! In this post, I've collected a variety of links related to major data center technology areas. This episode is a bit long; sorry about that!

## Networking

* Lindsay Hill [recently noted][link-2] that he's been working to add support to [netmiko][link-23] for the Brocade ICX and MLXe, and is looking into support for VDX. Netmiko, if you haven't heard, is a fantastic Python library that's really useful when writing Python-based network automation scripts.
* I mentioned a while back that I was taking a deeper look at MPLS (to which my colleague Bruce Davie---one of the creators of MPLS---jokingly quipped, "Why are you looking at legacy tech?"). Honestly, I haven't had a great deal of time to make much progress, but I did come across [this article by Sudeep Goyal][link-3] which helped reinforce some of the basics I already knew. It may prove useful to others who are also seeking to improve their knowledge of MPLS.
* Peter Phaal has been writing some _really_ interesting stuff (interesting to me, at least). First up, there's a great article on [using IPVLAN with Docker and Cumulus Linux][link-4] (with a tie back to sFlow, naturally!). I'm really eager to try this out myself, assuming I can ever free up enough time in my schedule. Next, Peter posted an article on [using sFlow for BGP route analytics][link-5]. This one is a bit farther beyond my skill set, but it's good to stretch yourself, right?
* Here's a walkthrough by Cody Bunch on [setting up BGP on Linux with Cumulus Quagga][link-6]. This is another setup I really want to try myself, but first I need to be more disciplined with my time management.
* Here's a pair of articles ([one here][link-15] and [one here][link-16]) that describe the port connections required between the various components in a VMware NSX deployment.
* Here's [a "cheat sheet" for working with Cumulus Linux][link-18]; it may prove useful for networking professionals who don't (yet) have a lot of familiarity with Linux. (If this is the case, I'd _highly_ recommend working on that.)
* Need to create an API-only user in NSX? See [this article][link-21] by Dale Coghlan.
* I recently ran into an issue where some of my servers were sending LLDP to the switches, but others were not. Andy Hill sent me [an article he wrote on troubleshooting LLDP][link-26]. Sadly, the fix for Andy (updating firmware) did not work for me, but Andy's troubleshooting steps may be helpful nevertheless.

## Servers/Hardware

* Frank Denneman has a great series on NUMA (the "NUMA Deep Dive"). So far, he's published [part 1][link-27], [part 2][link-28], [part 3][link-29], and [part 4][link-30]. Three more posts are planned. These posts are a bit slanted toware VMware vSphere environments, but very detailed, very thorough, and very useful.
* Kevin Houston has [an updated blade server comparison chart][link-31] that might be helpful in making hardware decisions.

## Security

* Via [an article by Jason Scanga][link-1], I found out that a STIG (security technical implementation guide) has been released for VMware NSX. Thanks Jason!
* Rob Hirschfeld has an article listing [some ways containers are more secure than VMs][link-25]. Some of the items listed are items I can genuinely agree with---restricting ports by default, for example. Others, particularly the 7 items listed as "best practices around containers" are less about containers and more about being proactive about security (many of these things could also apply to VMs as well as containers).

## Cloud Computing/Cloud Management

* Sandeep Kaushik and Shaswati Mukherjee have an article outlining [the VRA appliance setup process][link-8]; this might be useful to those of you out there who are wanting to evaluate or deploy VRA.
* VMware is offering licenses for Log Insight to customers who already own NSX. Steve Flanders has more details in [his blog post][link-13]. (By the way, be sure to follow Steve for all sorts of Log Insight-related goodness.)
* I have to say, this is a pretty smart move: a couple weeks ago, Amazon announced that the EC2 Run command---which allows you to execute commands on instances---has been extended to also support servers running _outside_ EC2, including servers running in your on-premises data center. You can get more details in [this blog post by Jeff Barr][link-14].
* William Lam has [a quick look at ContainerX on VMware vSphere][link-17].
* Paul Duvall has a great article on [using AWS CloudFormation to automate ECS (EC2 Container Service)][link-19]. The article includes a pretty fair amount of sample CloudFormation code, which is---to me, at least---very helpful in understanding how everything fits together. ([Part 2][link-20] is pretty good, too, but more focused on developer-centric services such as CodePipeline.)
* Looks like I'm not the only fan of mind maps out there...Diego Casati has published [a mind map of the OpenStack CLI cheat sheet][link-22].
* How about [some icons for vRA/vCAC][link-24]?

## Operating Systems/Applications

* Brendan Burns---who recently left Google to join the Microsoft Azure team---and David Oppenheimer published [an article on emerging container design patterns][link-7] they're seeing in Kubernetes environments. The article also has a link to a paper that Burns will present at HotCloud '16.
* Cormac Hogan wrote up an article describing [how to prepare Photon OS for deploying frameworks on Photon Controller][link-9]. The process isn't complicated, but there are a fair number of steps---this is a reflection, as Cormac noted, of the desire to keep Photon OS as minimal as possible.
* Cormac also wrote an article describing [a pre-release Photon Controller driver for Docker Machine][link-10], which allows you to spin up Docker Engine instances on Photon Controller. At this point, you'll still need to manually compile Docker Machine and the Photon Controller plugin, so if you're not comfortable with that you may want to wait a bit.
* René Moser describes [how to manage BIND and DNS zone files using Ansible][link-11].
* One of the announcements at DockerCon '16 was the Distributed Application Bundle (DAB), a way to package up multiple containers. [This Docker blog post][link-12] provides more details on DABs.

## Storage

* I found a great couple of articles from Robin Harris around "Hitchhiker," an improvement to advanced erasure coding. [The first article][link-33] describes Hitchhiker itself, while [the second article][link-34] discusses the related topic of the network impact of recovery in erased coded storage environments.
* Want to get geeky with [storage protocol stacks][link-35]?

## Virtualization

* Here's a really good article from William Lam on [disabling vMotion, Storage vMotion, and even Cross vCenter vMotion for a particular VM][link-32]. I know it sounds strange to _want_ to disable valuable features such as these, but as William notes in the article there are definitely valid use cases out there.

## Career/Soft Skills

Nothing this time around, but I'll keep my eyes peeled for content I can include in a future installation!

OK, I'd better wrap this up before it gets any longer. (I've already exceeded my "generally don't include more than this number of links" threshold.) Hopefully something I've listed here will prove useful to you. Until next time!



[link-1]: http://www.vm5280.com/2016/07/new-vmware-nsx-dod-stig-released.html
[link-2]: http://lkhill.com/netmiko-support-for-brocade-icx-and-mlxe/
[link-3]: http://mplstutorial.com/mpls-basics
[link-4]: http://blog.sflow.com/2016/06/docker-networking-with-ipvlan-and.html
[link-5]: http://blog.sflow.com/2016/07/real-time-bgp-route-analytics.html
[link-6]: http://blog.codybunch.com/2016/07/12/Getting-started-with-BGP-on-Linux-with-Cumuls-Quagga/
[link-7]: http://blog.kubernetes.io/2016/06/container-design-patterns.html
[link-8]: http://www.dtechinspiration.com/vmware-vrealize-automation-vra-appliance-setup/
[link-9]: http://cormachogan.com/2016/06/08/preparing-photon-os-deploying-on-photon-controller/
[link-10]: http://cormachogan.com/2016/06/17/docker-machine-photon-controller/
[link-11]: http://renemoser.net/blog/2014/04/28/manage-bind-and-zones-files-using-ansible/
[link-12]: https://blog.docker.com/2016/06/docker-app-bundle/
[link-13]: http://sflanders.net/2016/06/14/log-insight-3-3-2-nsx/
[link-14]: https://aws.amazon.com/blogs/aws/ec2-run-command-update-hybrid-and-cross-cloud-management/
[link-15]: http://vcrooky.com/2016/06/nsx-firewall-port-requirements/
[link-16]: http://www.virten.net/2016/05/vmware-nsx-6-component-communication-diagram/
[link-17]: http://www.virtuallyghetto.com/2016/06/test-driving-containerx-on-vmware-vsphere.html
[link-18]: https://community.cumulusnetworks.com/cumulus/topics/cumulus-linux-cheatsheet
[link-19]: https://stelligent.com/2016/05/26/automating-ecs-provisioning-in-cloudformation-part-1/
[link-20]: https://stelligent.com/2016/06/10/automate-amazon-ec2-container-service-provisioning-and-orchestration-using-cloudformation-and-aws-codepipeline/
[link-21]: http://www.sneaku.com/2016/01/22/how-to-create-a-nsx-v-api-user-account/
[link-22]: https://diegocasati.com/2016/06/27/mind-map-openstack-cli/
[link-23]: https://github.com/ktbyers/netmiko
[link-24]: http://www.vmtocloud.com/vravcac-icon-pack/
[link-25]: http://thenewstack.io/thirteen-ways-containers-secure-virtual-machines/
[link-26]: https://virtualandy.wordpress.com/2015/04/07/troubleshooting-lldp/
[link-27]: http://frankdenneman.nl/2016/07/07/numa-deep-dive-part-1-uma-numa/
[link-28]: http://frankdenneman.nl/2016/07/08/numa-deep-dive-part-2-system-architecture/
[link-29]: http://frankdenneman.nl/2016/07/11/numa-deep-dive-part-3-cache-coherency/
[link-30]: http://frankdenneman.nl/2016/07/13/numa-deep-dive-4-local-memory-optimization/
[link-31]: http://bladesmadesimple.com/2016/06/blade-server-comparison-june-2016/
[link-32]: http://www.virtuallyghetto.com/2016/07/how-to-easily-disable-vmotion-cross-vcenter-vmotion-for-a-particular-virtual-machine.html
[link-33]: http://storagemojo.com/2016/07/11/building-fast-erasure-coded-storage/
[link-34]: http://storagemojo.com/2016/07/12/bandwidth-reduction-for-erasure-coded-storage/
[link-35]: http://brasstacksblog.typepad.com/brass-tacks/2016/07/storage-protocol-stacks.html
