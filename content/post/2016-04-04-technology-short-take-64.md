---
author: slowe
categories: Information
comments: true
date: 2016-04-04T00:00:00Z
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- Linux
- Intel
- AWS
- VMware
- vSphere
- Docker
- LXC
- Consul
- Fusion
- Messaging
- Productivity
title: 'Technology Short Take #64'
url: /2016/04/04/technology-short-take-64/
---

Welcome to Technology Short Take #64. Normally, I try to publish Short Takes on Friday, but this past Friday was April Fools' Day. Given the propensity for "real" information to get lost among all the pranks, I decided to push this article back to today. Unlike most of what is published around April Fools' Day, hopefully everything here is helpful, informative, and useful!

## Networking

* Phil Gervasi has an article on [fate sharing in the network core][link-1], where he discusses some of the advantages---and disadvantages---of centralizing the control plane in network designs. The examples Phil uses include StackWise, Virtual Switching System, and Virtual Port Channels.
* Alban Crequy has a post on using Linux queueing disciplines (qdiscs, for short) to help with [testing application behavior under degraded network conditions][link-6]. This is really interesting and something that I want to explore in more detail.
* Cumulus Networks has added another hardware partner to its list of supported hardware partners; this time it's Mellanox, as outlined in [this SDx Central article][link-7].
* Carlos Cardenas has a great post that does a great job of [explaining SAI (Switch Abstraction Interface) and switchdev][link-8], two key abstraction layers involved in building Linux-based network operating systems (NOSes). Carlos also has a good article [taking a deeper look at Microsoft SONiC][link-13].
* Among a _lot_ of other great content, Russ White has three (so far) articles on the design mindset ([part 1][link-31], [part 2][link-32], [part 3][link-33]). While Russ' articles are written from a network engineering perspective---hence why I'm putting them in this section---the underlying principles he's discussion could be just as easily applied to any sort of IT design project. Highly recommended reading!

## Servers/Hardware

* Kevin Houston has [a write-up on the recent Intel "Broadwell" announcement][link-34] that provides details on the latest CPU family.

## Security

* Andreas Wittig reminds people of the common-sense [security rules regarding SSH key pairs][link-22], specifically with Amazon EC2, and provides a few suggestions for how to address shared key pairs. Personally, I would have liked to see much more detail on the suggested workarounds.

## Cloud Computing/Cloud Management

* Rodney Haywood has a great post that shows how to use my new favorite tool `jq` with the AWS pricing file to be able to determine [which instances are available in your desired region][link-4]. Good stuff.
* VMware recently released version 0.8 of Photon Controller, and James Zabala has [a blog post describing the new Photon Controller release][link-9]. Photon Controller is open source and available [from GitHub][link-10], so feel free to give it a spin.
* The latest buzzword to hit the industry is "serverless architecture." Now, we all know that cloud is just someone else's computer, and that there's no such thing as a truly serverless architecture (there are _going_ to be servers somewhere). Paul Johnston tackles [the definition of serverless architecture][link-16] in this post, and I think it's worth a read.
* Hassan Hamade from VMware has [an in-depth post on vRA network profiles][link-21] that might be helpful if you're working with vRA and NSX.

## Operating Systems/Applications

* Sebastien Goasguen has a decent [introductory article to using Docker Compose][link-2]. If you're new to Compose, this might be a good place to start. After you've read Sebastien's introductory article, then take it to the next level by combining Compose with Docker Machine and Docker Swarm in [this post][link-3] by Arun Gupta.
* Florian Haas of Hastexo takes on the current model of container deployments in a post that describes [a new approach using LXC in combination with OverlayFS][link-12]. It's a very interesting model that could offer some great benefits, and it's an approach that I hope to be able to try out myself soon.
* Although Docker Swarm 1.1.0 added native rescheduling of containers on failures of Swarm nodes, there are still some challenges in leveraging this functionality. Maximilian Schöfmann describes some of these challenges in [this blog post][link-15].
* If you're a total beginner and trying to come up to speed on containers, VMs, and Docker, read [this post][link-17]. Preethi does a good job of breaking down terminology and concepts, specifically targeting folks who are new to the technology.
* LogPacker has a couple of blog posts (two posts so far) talking about service discovery with [Consul][link-20] ([part 1][link-18] and [part 2][link-19]). Consul and service discovery with Consul are things I've discussed before (see [here][xref-3] and [here][xref-4]), but the LogPacker articles provide a good overview of the terms and concepts involved.
* Project Platypus looks interesting. What is Platypus? In the words of Paul Gifford, it's an effort "to provide a Swagger based documentation of several VMware Products". Check out [this project update][link-28] and [this example of working with Platypus][link-29].

## Storage

* Karim Vaes has a post describing [various storage patterns for persistence with Docker containers][link-14]. It's a good post for those seeking to solidify their understanding of how to handle persistence in a Docker-centric environment.
* Now that ScaleIO 2.0 supports Ubuntu (see [Chad's article][link-35]), I may have to give it a closer look.

## Virtualization

* I can't recall if I've mentioned this before, but just for the sake of completeness I'll include it here again: the VMware Host Client, a HTML5-based UI used to connect and manage single ESXi hosts, has reached version 1.0 and is now GA as part of vSphere 6.0 U2. More information is available [here][link-11]. Nice!
* Finally, [an HTML5-based vSphere Web Client][link-23]. Go get it. Now.
* Jon Benedict (aka "Captain KVM") blogged recently about [hot adding CPUs in RHEV][link-24]. I wasn't aware that RHEV supported hot-adding CPUs; I thought this was only a feature of vSphere/ESXi and Hyper-V. Anyone know if this will make it into the underlying open source projects underneath RHEV, or is the functionality already there?
* Anthony Burke is flexing his sysadmin skills (he comes from a predominantly networking-centric background) by [using PowerCLI to upgrade vSphere][link-25].
* William Lam worked with one of the VMware engineers, Songtao Zheng, to get an (unsupported) USB 3.0 Ethernet adapter driver working for ESXi 5.5 and 6.0. Read more about it [in William's blog post][link-26]. Also, check out William's [article on using virtual serial ports][link-27] as a way of capturing nested ESXi troubleshooting information.
* Pete Long has a good article on [converting a VirtualBox VM to VMware Fusion][link-30].

## Career/Soft Skills

* Tyler Britten takes a different approach to e-mail management with [his "Inbox - Meh" approach][link-5]. Honestly, his approach isn't terribly different from what I described in 2013 (see [here][xref-1] and [here][xref-2]). I'd say don't focus so much on "getting to zero"; instead, focus on _processing_ your e-mail so that you can get on with your real work.

It's time to wrap up now. See you again soon!



[link-1]: http://networkphil.com/2016/03/18/fate-sharing-in-the-network-core/
[link-2]: http://www.linux.com/learn/tutorials/893685-introduction-to-docker-compose-tool-for-multi-container-applications
[link-3]: http://blog.arungupta.me/docker-machine-swarm-compose-couchbase-wildfly/
[link-4]: http://rodos.haywood.org/2016/03/which-instances-are-available-in-my.html
[link-5]: http://vmtyler.com/email-management-inbox-meh/
[link-6]: https://kinvolk.io/blog/2016/02/testing-degraded-network-scenarios-with-rkt/
[link-7]: https://www.sdxcentral.com/articles/news/mellanox-lets-cumulus-linux-ride-its-ethernet-switches/2016/03/
[link-8]: http://packetpushers.net/sai-and-switchdev-need-to-succeed/
[link-9]: http://blogs.vmware.com/cloudnative/photon-controller-v08
[link-10]: https://github.com/vmware/photon-controller
[link-11]: http://blogs.vmware.com/vsphere/2016/03/vmware-host-client-1-0-now-ga.html
[link-12]: https://www.hastexo.com/blogs/florian/2016/02/21/containers-just-because-everyone-else/
[link-13]: http://packetpushers.net/people-getting-sonic-wrong/
[link-14]: https://kvaes.wordpress.com/2016/02/11/docker-storage-patterns-for-persistence/
[link-15]: http://container-solutions.com/rescheduling-containers-on-node-failures-with-docker-swarm-1-1/
[link-16]: https://medium.com/@PaulDJohnston/what-is-serverless-architecture-43b9ea4babca#.7s8kvnup2
[link-17]: https://medium.freecodecamp.com/a-beginner-friendly-introduction-to-containers-vms-and-docker-79a9e3e119b#.ya26ob45y
[link-18]: https://logpacker.com/blog/consul-service-discovery-1
[link-19]: https://logpacker.com/blog/consul-service-discovery-2
[link-20]: https://www.consul.io/
[link-21]: http://blogs.vmware.com/management/2016/03/demystifying-vrealize-automation-network-profiles.html
[link-22]: https://cloudonaut.io/avoid-sharing-key-pairs-for-ec2/
[link-23]: https://labs.vmware.com/flings/vsphere-html5-web-client
[link-24]: http://captainkvm.com/2016/03/hot-adding-cpus-rhev/
[link-25]: http://networkinferno.net/use-powercli-to-upgrade-vsphere
[link-26]: http://www.virtuallyghetto.com/2016/03/functional-usb-3-0-ethernet-adapter-nic-driver-for-esxi-5-5-6-0.html
[link-27]: http://www.virtuallyghetto.com/2016/03/vm-serial-logging-to-the-rescue-for-capturing-nested-esxi-psod.html
[link-28]: http://dailyhypervisor.com/platypus-project-update/
[link-29]: http://www.cloudcanuck.ca/posts/working-with-project-platypus
[link-30]: http://www.petenetlive.com/KB/Article/0001169
[link-31]: http://ntwrk.guru/design-mindset-1/
[link-32]: http://ntwrk.guru/design-mindset-2/
[link-33]: http://ntwrk.guru/design-mindset-3/
[link-34]: http://bladesmadesimple.com/2016/03/intel-releases-broadwell-cpus-for-servers/
[link-35]: http://virtualgeek.typepad.com/virtual_geek/2016/03/scaleio-20-the-march-towards-a-software-defined-future-continues.html
[xref-1]: {{< relref "2013-06-21-reducing-the-friction-processing-e-mail.md" >}}
[xref-2]: {{< relref "2013-07-19-reducing-the-friction-processing-e-mail-part-2.md" >}}
[xref-3]: {{< relref "2015-02-06-quick-intro-to-consul.md" >}}
[xref-4]: {{< relref "2015-02-28-experimenting-docker-registrator-consul.md" >}}