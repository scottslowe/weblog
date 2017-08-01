---
author: slowe
categories: Information
comments: true
date: 2015-11-17T00:00:00Z
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- OpenStack
- OVS
- OVN
- NSX
- Ansible
- CoreOS
- Docker
- VMware
- VSAN
- vSphere
title: 'Technology Short Take #56'
url: /2015/11/17/technology-short-take-56/
---

Welcome to Technology Short Take #56! In this post, I've collected a few links on various data center technologies, news, events, and trends. I hope you find something useful here.

## Networking

* Open Virtual Network (OVN) is really ramping up and getting lots of attention, which I personally think is absolutely well-deserved. Russell Bryant has a couple great articles on OVN---[how to test OVN's "EZ Bake" release with DevStack][link-3] as well as an article on [implementing OpenStack security groups using OVN ACLs][link-4] (which in turn leverage the integration between Open vSwitch and the Linux kernel's conntrack module). Gal Sagie has a write-up on [integration between Kuryr and OVN][link-5] (Kuryr is another topic that is really interesting).
* Here's an article on [using PHP to query NSX API via REST][link-20] (specifically, working with syslog settings on NSX Manager).
* Speaking of Gal Sagie and OVN, Gal has [a post describing a concept for something called "Topology Service Injection"][link-21] and some proposals for implementing this in OVN. (Gal and Liran Schour from IBM are slated to do a talk on this at the OVS conference this week.)
* One new networking feature added to OpenStack in the Kilo release was Neutron subnet pools. Carl Baldwin has [a post][link-33] describing how subnet pools work and why they are of benefit in OpenStack environments.

## Servers/Hardware

* Sean Collins has an article on building [a cheap, compact, and multinode DevStack environment for a home lab][link-27] that lays out some server hardware decisions and the tools he uses to manage them.

## Security

* The security posture of Docker containers is, rightfully so, starting to see more focus. A couple of articles jumped out at me while compiling this Technology Short Take post. First, [this post from Red Hat on Deep Container Inspection (DCI)][link-18] talks about how DCI's goal is to allow users to verify where the image came from as well as verify what's inside the image. Second, CoreOS recently announced [Clair][link-19], their new container vulnerability analysis service designed to work hand-in-hand with their registry, Quay.io. (More on Clair from TechCrunch [here][link-28].)
* Major Hayden has an article on [using Ansible to secure OpenStack hosts][link-23]. This effort is aimed at implementing the RHEL 6 STIG (available [here][link-24]) on Ubuntu 14.04.

## Cloud Computing/Cloud Management

* I recently had to use DevStack for a demonstration of OVN and OpenStack. This was my first "real" use of DevStack, but others who have been using DevStack far more than I have are starting to explore new ways of running OpenStack environments instead of using DevStack. One such method is described by Miguel Grinberg in [his article titled "Life Without DevStack: OpenStack Development with OSA,"][link-8] in which he discusses using Ansible to deploy OpenStack instances. (Miguel also gave a talk at the OpenStack Tokyo Summit on this topic.)
* I've talked about OpenStack Heat and Heat templates a few times ([here][xref-2], [here][xref-3], and [here][xref-4], for example), but I recently came across [another introduction to Heat templates][link-36] that might provide a different approach to the topic.

## Operating Systems/Applications

* In case you missed it, Docker 1.9 was recently released, and along with it came production-ready Docker Swarm and the much-anticipated Docker Networking. See [the official Docker blog post][link-12] for more information (and rest assured I'll have some blog posts up on some of this stuff as well).
* Linux network namespaces is a topic I've covered [here before][xref-1], but it's always great to have multiple viewpoints and explanations of technologies and concepts to get a complete and comprehensive view. Jon Langemak has [a write-up on network namespaces][link-1] as well that is worth reading. Matt Oswalt also [tackled the topic of network namespaces][link-35] recently as well.
* Here's an article on [how to customize Docker's `docker0` network bridge][link-2] (Bill throws in a rant about said topic for free).
* [This article][link-6] provides a reasonable overview (well-suited for beginners or folks new to the technologies) of the various container orchestration tools like Swarm, Kubernetes, Fleet, and Mesos.
* Articles such as [this one from Barricade][link-9] that describe the contents of a modern infrastructure stack can be immensely helpful, if for no other reason than get a look at the technologies that are gaining popularity with newer organizations. I recommend you read [articles like this][link-10], and use the products and projects listed there to help you navigate where you're headed.
* Docker and Solaris Zones? [Interesting combination][link-14].
* William Lam shares an article on [using Ansible to provision Kubernetes on VMware Photon][link-16]. I've been looking into Kubernetes, but I hadn't considered the use of Ansible with Photon. (Mistakenly, I considered that Python, which is required for Ansible, had been stripped from Photon.) This is something I'll have to investigate a bit more.
* Lew Goettner has [a pretty hefty post on CoreOS and Docker on AWS][link-25] that includes information on CoreOS, user data and cloud-init, AWS and Elastic Load Balancers (ELBs), Fleet, Registrator, Nginx, Confd, and Jenkins. It's a whirlwind of technologies. Be sure to set aside some time to really focus on this article; there's a lot of depth here.
* Nathan LeClaire has a post on [using Ansible with Docker Machine to bootstrap host nodes][link-26]. It's an interesting approach in that he uses Ansible _in a container_ to provision the host. This is something I'll need to review again and digest a bit further.
* With KubeCon last week, there was naturally a fair amount of news surrounding Kubernetes. Engine Yard (and Deis, acquired by Engine Yard) [announced a packaging service][link-29] called Helm (more info on Helm [here][link-30]), and Sysdig Cloud announced [the ability to monitor Kubernetes clusters][link-31]. (If you're not familiar with Kubernetes, be sure to check out Matt Oswalt's post on [basic concepts for Kubernetes][link-32].)

## Storage

* Christian Mohn shares an epiphany he had about [the possible future of VSAN in this post][link-13]. Is VMware headed to turning VSAN into a generic storage platform that is no longer tied to vSphere? I don't know, but it's certainly an interesting thought.

## Virtualization

* Brian Graf provides an update on using VMware Tools 10 with [this "must-read" article on updating to VMware Tools 10][link-15].
* Here's a handy trick of the new ESXi Embedded Host Client: it turns out you can install or update any VIB. See [this post][link-22] from William Lam.
* Here's an interesting use of Docker [to package the Ruby vSphere Console][link-34] (RVC) to make it easier for vSphere admins to use the tools in the RVC.

## Career/Soft Skills/Productivity

* Cody Bunch has a nice article (part of his larger "vSensei" efforts) that bounces around [a few common themes on finding/making time][link-7] for personal development, additional projects, etc.
* For those of you considering pursuing the VCDX (an admirable goal, by the way), be sure to have a look at Gregg Robertson's article on [his journey to VCDX #205][link-11].
* Speaking of the journey to VCDX...if you're an existing VCDX and **not** a panelist, perhaps you might consider [being a VCDX mentor][link-17].

I'd better wrap this up now, before it gets any longer (it's already long enough!). I'll have more links, articles, and posts for you next time around. Until then, thanks for reading!



[link-1]: http://www.dasblinkenlichten.com/an-introduction-to-network-namespaces/
[link-2]: https://blog.plein.org/2015/08/11/how-to-customize-docker0-network-options-in-docker/
[link-3]: http://blog.russellbryant.net/2015/05/14/an-ez-bake-ovn-for-openstack/
[link-4]: http://blog.russellbryant.net/2015/10/22/openstack-security-groups-using-ovn-acls/
[link-5]: http://galsagie.github.io/sdn/openstack/docker/kuryr/neutron/2015/10/10/kuryr-ovn/
[link-6]: http://radar.oreilly.com/2015/10/swarm-v-fleet-v-kubernetes-v-mesos.html
[link-7]: http://blog.codybunch.com/2015/11/02/vSensei-Finding-Time/
[link-8]: https://developer.rackspace.com/blog/life-without-devstack-openstack-development-with-osa/
[link-9]: https://blog.barricade.io/running-a-modern-infrastructure-stack/
[link-10]: https://segment.com/blog/rebuilding-our-infrastructure/
[link-11]: http://thesaffageek.co.uk/2015/10/30/vcdx-205/
[link-12]: https://blog.docker.com/2015/11/docker-1-9-production-ready-swarm-multi-host-networking/
[link-13]: http://vninja.net/virtualization/vmware-vsan-more-than-meets-the-eye/
[link-14]: https://blog.docker.com/2015/08/docker-oracle-solaris-zones/
[link-15]: https://blogs.vmware.com/vsphere/2015/11/updating-to-vmware-tools-10-must-read.html
[link-16]: http://www.virtuallyghetto.com/2015/11/using-ansible-to-provision-a-kubernetes-cluster-on-vmware-photon.html
[link-17]: http://www.simonlong.co.uk/blog/2015/11/03/vcdx-mentors-we-need-you/
[link-18]: http://rhelblog.redhat.com/2015/09/03/what-is-deep-container-inspection-dci-and-why-is-it-important/
[link-19]: https://github.com/coreos/clair
[link-20]: http://www.sneaku.com/2015/11/09/using-php-to-query-nsx-v-via-rest/
[link-21]: http://galsagie.github.io/sdn/nfv/openstack/ovs/dragonflow/2015/11/10/topology-service-injection/
[link-22]: http://www.virtuallyghetto.com/2015/11/neat-way-of-installing-or-updating-any-vib-using-just-the-esxi-embedded-host-client.html
[link-23]: http://www.ansible.com/blog/securing-openstack-hosts-with-ansible
[link-24]: http://iase.disa.mil/stigs/os/unix-linux/Pages/index.aspx
[link-25]: https://www.goettner.net/2015/coreos-and-docker-on-aws-revised/
[link-26]: http://nathanleclaire.com/blog/2015/11/10/using-ansible-with-docker-machine-to-bootstrap-host-nodes/
[link-27]: http://coreitpro.com/2015/11/11/devstack-home-lab-pt1.html
[link-28]: http://techcrunch.com/2015/11/13/coreos-launches-clair-an-open-source-tool-for-monitoring-container-security/
[link-29]: http://sdtimes.com/engine-yard-container-group-launches-kubernetes-packaging-system/
[link-30]: http://helm.sh
[link-31]: https://sysdig.com/monitoring-kubernetes-with-sysdig-cloud/
[link-32]: http://keepingitclassless.net/2015/11/kubernetes-basic-concepts/
[link-33]: http://blog.episodicgenius.com/post/neutron-subnet-pools/
[link-34]: http://www.virtuallyghetto.com/2015/11/docker-container-for-the-ruby-vsphere-console-rvc.html
[link-35]: http://keepingitclassless.net/2015/10/namespaces-new-access-layer/
[link-36]: http://captainkvm.com/2015/11/intro-to-heat-templates/
[xref-1]: {{< relref "2013-09-04-introducing-linux-network-namespaces.md" >}}
[xref-2]: {{< relref "2014-05-01-an-introduction-to-openstack-heat.md" >}}
[xref-3]: {{< relref "2014-05-02-another-look-at-an-openstack-heat-template.md" >}}
[xref-4]: {{< relref "2015-04-23-ubuntu-openstack-heat-cloud-init.md" >}}
