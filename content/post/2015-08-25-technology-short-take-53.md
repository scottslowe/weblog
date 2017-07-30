---
author: slowe
categories: Information
comments: true
date: 2015-08-25T11:15:00Z
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- Docker
- NSX
- Ansible
- VMware
- OpenStack
- Linux
- Vagrant
- vSphere
title: 'Technology Short Take #53'
url: /2015/08/25/technology-short-take-53/
---

Welcome to Technology Short Take #53. In this post, I've gathered links to posts about networking, virtualization, Docker, containers, Linux, configuration management, and all kinds of other cool stuff. Here's hoping you find something useful!

## Networking

- Anthony Spiteri, who works at an Australian service provider running NSX, has some in-depth articles discussing vShield Edge and NSX Edge ([part 1][link-1], [part 2][link-2], [part 3][link-3], and [part 4][link-4]). Anthony discusses features supported by both, how they handle high availability, how VPN services work, and how to handle certificates. It looks like very useful information for anyone supporting NSX in their environment.
- Here's a nice article on [using Ansible with Arista EOS][link-6]. This isn't something I've had the chance to do just yet (currently needing to focus my efforts on Ansible with Linux/Cumulus Linux), but it certainly seems relatively straightforward. Without having done this myself, it seems like I'd prefer to run `pyeapi` on the switches directly, so the Ansible configuration remains clean (instead of having to use a local connection for the switches but SSH for everything else). Of course, I'm sure there are trade-offs each way.
- I think I've mentioned this before (it's really hard to keep track of which articles you've included in a Technology Short Take already, so apologies if this is a duplicate), but this article provides a good overview of [the VXLAN control plane modes in VMware NSX][link-7].
- I think Brent Salisbury is going to be the "star" of this Technology Short Take, since I've got several articles of his that I want to reference. (Keep up the great work, Brent!) If you haven't read Brent's article on [building network tools with Docker][link-16], I _highly_ recommend it. The first part of this article does a great job of describing some of the key forces that are shaping the networking industry. Brent is one of the folks who clearly sees that the role of a networking professional is changing, and is working to help others through the transition.

## Servers/Hardware

Nothing this time around, but I'll keep my eyes peeled for content to include in future posts.

## Security

- Josh Townsend discusses [the use of vShield Endpoint with vSphere 6.0][link-5], including addressing some questions around how the vCNS End of Availability/End of Support announcements.
- Roie Ben Haim, who works in professional services at VMware, has a [deep dive on the NSX distributed firewall (DFW)][link-11]. I haven't had a chance to read it all, but it seems pretty comprehensive.

## Cloud Computing/Cloud Management

- Here's a very practical post from Maish Saidel-Keesing (not that his other posts aren't practical!) on [downloading the videos from the OpenStack Summit][link-8]. Very useful if you (like me) couldn't make it to the last Summit in Vancouver.
- Having spent a bit of time using Docker Machine, I find it to be a very interesting tool. I don't see it replacing other tools (just like I don't necessarily see Linux containers replacing virtualization or bare metal), but I _do_ think it's a nice complement to existing tools. If you're interested in learning more about Docker Machine, here's a couple of posts you might find useful. First, Nathan LeClaire has a [Docker Machine 0.3.0 (latest version) deep dive][link-14] that is useful. Second, Brent Salisbury has a write-up on [using Docker Machine with AWS][link-15] that provides a useful real-world example of how it might be used.
- Cody Bunch has [a short (but sweet) post][link-19] on how using `depends_on` in OpenStack Heat templates allows you to specify the start-up order of instances created by that template. Simple, but effective.

## Operating Systems/Applications

- This is probably more "just for fun" than for anything else, but it's worth including as well. Jessie Frazelle has done some pretty amazing things with Docker containers (she did a session---which I unfortunately missed---on using Docker containers for desktop Linux apps that I heard was fabulous), and in this post she talks about [how to route traffic through a Tor Docker container][link-9].
- For an alternative to the "Rah rah Docker is the best tool EVAR" mentality, I invite you to peruse this article on [why Docker is not yet succeeding widely in production][link-18]. Simon's article is, I think, a well-balanced view of the positives _and_ the negatives that coming from using Docker at scale in production.
- Jason Anderson has a nice post on [using SR-IOV (Single Root I/O Virtualization) to expose Docker containers][link-10]. The gist is that you can use SR-IOV to supply each Docker container with its own "dedicated" NIC (which is really just a virtual function on the actual physical NIC). This is pretty cool, but does have some limitations; specifically, the number of virtual functions supported on the physical NIC (in Jason's article, the limit was 63). Thus, this approach may only be viable for a limited number of Docker containers on a container host. It's also worth noting that you have to "wrap" the Docker command using a tool like `pipework` in order to make this work. (It would be interesting to see/know if the upcoming Docker Network will address this sort of use case.)
- Cloud Foundry is undergoing some changes to evolve along with the rapid rise of containerization; [this post][link-20] on Garden (CF's containerization layer) and runC (the new container runtime from the Open Container Initiative) provides some details on the direction the project is headed.
- CenturyLink Labs has [a good article][link-21] on effectively using `docker inspect` to gather information about Docker images and running containers.
- Check out [this packaging of the vCloud Air CLI as a Docker container][link-23]. Handy.
- This article by Michael Gugino provides some details on [getting GRE tunnels over IPv6 with Open vSwitch running on CentOS 7][link-24]. Thanks Mike!
- Neowin has a quick recap of [what's new in Windows Server 2016 Technical Preview 3][link-26], if you're interested in seeing what's happening on that front.

## Storage

- John Griffith has a blog post (slightly older, from December 2014) on [using OpenStack live migration with Cinder-backed instances][link-17].

## Virtualization

- VMware unveiled AppCatalyst at DockerCon 2015, and some AppCatalyst materials have started to pop up here and there. First, here's [a "first impressions" post][link-12] on AppCatalyst that also provides a few additional links to other AppCatalyst posts. Once you're done reading that, have a look at this post on [running lattice.cf on AppCatalyst][link-13] for a more in-depth look at using AppCatalyst.
- Ryan Kelly has a great article on [using VMware vSphere, Vagrant with the vSphere provider, and VMware Photon together][link-22] to enable developers to leverage the infrastructure you already have in place. This is, IMHO, a _great_ way to start enabling developers to use tools that may feel comfortable to them while still taking advantage of the investments your organization has already made in a VMware vSphere-based environment. The only thing I'd like to see on top of this is using Docker Machine and/or Docker Compose along with this setup...hmmm...might be something I need to explore!
- Mark Russinovich, CTO for Microsoft Azure, has [a long but informative article on the union of Docker containers, Windows Server, and Hyper-V][link-25]. The article provides a good overview of containers, reminds folks that although Windows Server will support the Docker APIs and the Docker client you won't be able to run Linux containers directly on Windows (or vice versa), and reiterates the importance of a hypervisor with containers in mixed trust zones.
- Seems like William Lam has been rocking-and-rolling on generating content. (Kudos to you, William!) Here's [an update on the nested ESXi VMware Tools Fling][link-27], a post on [the new HTML5 Embedded Host Client][link-28], and [a list of some best practices/troubleshooting tips for the new Instant Clone PowerCLI cmdlets][link-29]. Very useful stuff!
- Ben Armstrong, aka "Virtual PC Guy," has decided to start sharing some of his scripts and code samples on GitHub (published with the MIT license). Nice! You can read about the move to GitHub and get more details in [his post here][link-30].

That's it for this time around. As always, I'd love to hear from you. Feel free [to hit me up on Twitter][link-31]. Thanks for reading!


[link-1]: http://anthonyspiteri.net/nsx-edge-vs-vshield-edge-part-1-feature-and-performance-matrix/
[link-2]: http://anthonyspiteri.net/nsx-edge-vs-vshield-edge-part-2-high-availability/
[link-3]: http://anthonyspiteri.net/nsx-edge-vs-vshield-edge-part-3-ipsec-and-l2-vpn/
[link-4]: http://anthonyspiteri.net/nsx-edge-vs-vshield-edge-part-4-generating-self-signed-ssl-certificates/
[link-5]: http://vmtoday.com/2015/05/vshield-endpoint-vsphere-6-0/
[link-6]: https://eos.arista.com/my-journey-with-ansible-and-arista/
[link-7]: https://telecomoccasionally.wordpress.com/2015/01/11/nsx-for-vsphere-vxlan-control-plane-modes-explained/
[link-8]: http://technodrone.blogspot.com/2015/06/downloading-all-sessions-from-openstack.html
[link-9]: https://blog.jessfraz.com/post/routing-traffic-through-tor-docker-container/
[link-10]: http://jason.digitalinertia.net/exposing-docker-containers-with-sr-iov/
[link-11]: http://www.routetocloud.com/2015/04/nsx-distributed-firewall-deep-dive/
[link-12]: https://dantehranian.wordpress.com/2015/06/30/vmware-appcatalyst-first-impressions/
[link-13]: http://bl.ocks.org/jrrickard/a2724c9ede44de0186cc
[link-14]: http://blog.docker.com/2015/06/docker-machine-0-3-0-deep-dive/
[link-15]: http://networkstatic.net/docker-machine-provisioning-on-aws/
[link-16]: http://networkstatic.net/building-network-tools-using-docker/
[link-17]: https://griffithscorner.wordpress.com/2014/12/08/openstack-live-migration-with-cinder-backed-instances/
[link-18]: http://sirupsen.com/production-docker/
[link-19]: http://blog.codybunch.com/2015/08/03/OpenStack-Heat-Specify-Boot-Order/
[link-20]: https://www.cloudfoundry.org/garden-and-runc/
[link-21]: https://labs.ctl.io/what-to-inspect-when-youre-inspecting/
[link-22]: http://www.vmtocloud.com/how-to-use-vagrant-to-deploy-containers-on-vsphere-with-vmware-photon/
[link-23]: http://vkohli13.blogspot.in/2015/08/vcloud-air-cli-as-docker-container.html
[link-24]: http://funwithlinux.net/2015/08/openvswitch-gre-over-ipv6-on-centos-7/
[link-25]: http://azure.microsoft.com/blog/2015/08/17/containers-docker-windows-and-trends/
[link-26]: http://www.neowin.net/news/whats-new-in-windows-server-2016-technical-preview-3
[link-27]: http://www.virtuallyghetto.com/2015/08/vmware-tools-for-nested-esxi-updated-to-v1-2.html
[link-28]: http://www.virtuallyghetto.com/2015/08/new-html5-embedded-host-client-for-esxi.html
[link-29]: http://www.virtuallyghetto.com/2015/08/instant-clone-powercli-cmdlets-best-practices-troubleshooting.html
[link-30]: http://blogs.msdn.com/b/virtual_pc_guy/archive/2015/07/20/virtual-pc-guy-on-github.aspx
[link-31]: https://twitter.com/scott_lowe
