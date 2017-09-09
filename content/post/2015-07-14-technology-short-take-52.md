---
author: slowe
categories: Information
comments: true
date: 2015-07-14T12:00:00Z
tags:
- Virtualization
- Storage
- Networking
- OVS
- Docker
- NSX
- VMware
- vSphere
- OpenStack
- Ansible
- Linux
title: 'Technology Short Take #52'
url: /2015/07/14/technology-short-take-52/
---

Welcome to Technology Short Take #52, the latest collection of news, links, and articles from around the web on data center technologies. 

## Networking

* Want to know a bit more about how OVN (Open Virtual Network) plans to integrate support for containers? See [this][link-2]. You might also find it useful to review [this OVN presentation][link-7] from the recent OpenStack Summit in Vancouver. A video recording of the presentation is [also available][link-5] on YouTube.
* QualiSystems has a series of articles on open networking standards. A couple of the articles really jumped out at me---[part 2][link-8] covers Open vSwitch, [part 3][link-9] discusses OpenStack, [part 4][link-10] discusses OpenFlow, and [part 6][link-11] talks about OVSDB. There are also posts on OpenDaylight and OpFlex as well.
* P4 is getting all the attention in the SDN world these days. What is P4? Craig Matsumoto has [an overview at SDx Central][link-14]; the "TL;DR" is that P4 is a high-level language aimed at describing how data plane devices process packets. If you want even more detail, then head over to [the P4.org site][link-15] for more information.
* Jason Edelman, whose focus has been on network automation, recently posted [an article on programming an ACI (Application Centric Infrastructure) fabric][link-16]. If you're into network programmability and automation, Jason is one of the folks whose content you should be following, IMHO.
* Michael Armstrong, an SE for VMware in the UK, has a write-up on [how to configure DHCP Relay in NSX 6.1][link-17].
* Dale Coghlan has a few useful NSX-related articles that I recently found. The first is a write-up on a script he wrote to [reset NSX objects to the defaults][link-18], which is extraordinarily useful if you're doing some testing. He also has [a script for importing Checkpoint objects into NSX][link-19], as well an article on [scripting the syslog server configurations on the NSX controllers][link-20]. There's a lot more too, so be sure to check out his site.

## Servers/Hardware

Nothing this time around, but I'll stay tuned for items to include next time!

## Security

* Running vSphere 6? Good news for you---the vSphere 6 hardening guide is now available! More details are found [here][link-23].

## Cloud Computing/Cloud Management

* [Here's a decent introduction][link-4] to "Magnum," the OpenStack Containers-as-a-Service project.
* Although slanted toward Network Functions Virtualization (NFV), this is [a good article][link-12] about why open source---and OpenStack, in particular---isn't necessarily "free."
* Michael DeHaan, creator of Ansible and Cobbler, has [a write-up on immutable infrastructure][link-13] that I think is _really_ worth reading. Lots of folks like to blather on and on about how immutable infrastructure is the future and how we all need to get there, but I like that Michael takes a very pragmatic approach:

    >There are easy steps.  First, make sure all data on your system can be externalized - use RDS or Cassandra.  Keep your logs off system.  Use something like logstash or Splunk.  Monitor externally.   Get to rolling updates with your classic automation.  Now just swap your classic automation out for an image build, that reproduces the configuration of your classic automation. You’re done.

    There you have it---some real, practical steps that can put you on track toward immutable infrastructure.
* Cormac Hogan recently started spending some time with VMware Integrated OpenStack (VIO), and shared some of the initial "lessons learned" on [this post about "adventures in VIO."][link-22] Thanks for sharing your experiences, Cormac!
* Speaking of VIO...interesting in trying it out, but don't have the resources in your (work or home) lab? No worries, Emad Younis has a write-up on [using Ravello to try out VMware Integrated OpenStack][link-24]. (If you're a VMware vExpert, you should read [this][link-26] to see how you can try this out yourself.)

## Operating Systems/Applications

* The 1.6 release of Docker introduces the idea of a logging driver, which should be very helpful in managing logging in heavily Dockerized environments. Sumo Logic has [a write-up on Docker logging drivers][link-3] that might be helpful.
* The Monsanto engineering blog recently published an article on using [etcd clustering with AWS Auto Scaling Groups][link-21]. The changes in etcd2 around bootstrapping and dynamic configuration made it possible to more easily manage dynamic etcd clusters on AWS. If you're interested in learning more about managing an etcd cluster, this is a good article to read.
* The list of concerns in [this article regarding the sad state of sysadmin][link-28] is one reason why I strongly advocate building something from scratch at first, so that you _learn_ how it works. Once you've learned how it works, then you can work on automating it.
* Here's [a nice write-up about Project Bonneville][link-29], which was announced during this year's DockerCon.
* There are a lot of "container-optimized" OSes floating around out there. Jonas Rosland takes [a quick stab at comparing some of them][link-30].

## Storage

* Containers may be all the rage in the IT industry these days, but Justin Warren [reminds us][link-27] that the value to many organizations (in his words) "lives in the _data_ your organization has". Thus, storage solutions for containers/containerized environments, while a bit sparse now, are likely to really start popping up very soon.

## Virtualization

* There's a lot of talk about how containers are going to replace "traditional" virtualization because of less overhead, better performance, etc. Some of the performance folks at VMware have been hard at work trying to quantify whether there is any substance to these claims. The latest set of tests involves [running transactional workloads using Docker containers on vSphere 6][link-1], but the article also contains links to some of the other sets of tests they've performed.
* William Lam, who always generates some great content, has an article on [automating the installation of VMware Tools on Linux with automatic kernel modules][link-25]. Good stuff.

## Career/Soft Skills

* While many folks think "DevOps = Technology," what they're overlooking is the cultural, personal, and procedural aspects. [This recent article][link-6] by John Willis (of Docker) that connects Docker with the "First Way of DevOps" (systems thinking) is helpful in that it serves to help folks see technology not as an end unto itself, but as a tool to achieve a goal (something I think a lot of people overlook/forget).

It's time to wrap up now. I hope that I was able to include something useful for you. In the meantime, stay tuned for the next Technology Short Take, and thanks for reading!



[link-1]: http://blogs.vmware.com/performance/2015/05/running-transactional-workloads-using-docker-containers-vsphere-6-0.html
[link-2]: http://openvswitch.org/pipermail/ovs-dev/2015-March/052663.html
[link-3]: https://www.sumologic.com/2015/04/16/new-docker-logging-drivers/
[link-4]: http://www.datacenterknowledge.com/archives/2015/05/22/openstack-magnum-containers-service-cloud-operators/
[link-5]: https://www.youtube.com/watch?v=kEzXTq2fPDg
[link-6]: http://blog.docker.com/2015/05/docker-three-ways-ops/
[link-7]: http://openvswitch.org/support/slides/OVN-Vancouver.pdf
[link-8]: http://www.qualisystems.com/blog/open-source-standards-for-network-orchestration-part-2-open-vswitch/
[link-9]: http://www.qualisystems.com/blog/open-source-standards-for-network-orchestration-part-3-openstack/
[link-10]: http://www.qualisystems.com/blog/open-networking-standards-and-the-road-to-agility-part-4-openflow/
[link-11]: http://www.qualisystems.com/blog/open-networking-standards-and-the-road-to-agility-part-6-ovsdb/
[link-12]: http://www.lightreading.com/nfv/nfv-specs-open-source/openstack-doesnt-come-for-free/a/d-id/715626
[link-13]: http://michaeldehaan.net/post/118717252307/immutable-infrastructure-is-the-future
[link-14]: https://www.sdxcentral.com/articles/news/p4-language-aims-to-take-sdn-beyond-openflow/2015/05/
[link-15]: http://p4.org
[link-16]: http://jedelman.com/home/programming-an-aci-fabric/
[link-17]: http://www.m80arm.co.uk/2014/10/configuring-dhcp-relay-in-nsx.html
[link-18]: http://www.sneaku.com/2015/06/15/scripting-resetting-nsx-v-objects/
[link-19]: http://www.sneaku.com/2015/02/06/scripting-nsx-v-importing-checkpoint-objects/
[link-20]: http://www.sneaku.com/2015/05/28/scripting-syslog-server-configurations-on-nsx-v-controllers/
[link-21]: http://engineering.monsanto.com/2015/06/12/etcd-clustering/
[link-22]: http://cormachogan.com/2015/06/02/adventures-in-vio-vmware-integrated-openstack/
[link-23]: http://blogs.vmware.com/vsphere/2015/06/vsphere-6-hardening-guide-ga-now-available.html
[link-24]: http://emadyounis.com/openstack/ravello-lab-setup-for-vmware-integrated-openstack-vio-part-1/
[link-25]: http://www.virtuallyghetto.com/2015/06/automating-silent-installation-of-vmware-tools-on-linux-wautomatic-kernel-modules.html
[link-26]: http://www.ravellosystems.com/blog/ravello-free-lab-vexperts/
[link-27]: http://www.forbes.com/sites/justinwarren/2015/07/06/the-container-revolution-much-ado-about-nothing/
[link-28]: http://www.vitavonni.de/blog/201503/2015031201-the-sad-state-of-sysadmin-in-the-age-of-containers.html
[link-29]: http://containerjournal.com/2015/07/01/vmwares-project-bonneville-streamlines-docker-container-workflow/
[link-30]: http://blog.codeship.com/container-os-comparison/
