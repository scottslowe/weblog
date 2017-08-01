---
author: slowe
categories: Information
comments: true
date: 2015-12-18T00:00:00Z
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- VMware
- NSX
- Docker
- Linux
- Microsoft
- OpenStack
- Vagrant
- OVS
- CoreOS
- SAN
- vSphere
title: 'Technology Short Take #58'
url: /2015/12/18/technology-short-take-58/
---

Welcome to Technology Short Take #58. This will be the last Technology Short Take of 2015, as next week is Christmas and the following week is the New Year's holiday. Before I present this episode's collection of links, articles, and thoughts on various data center technologies, allow me to first wish all of my readers a very merry and very festive holiday season. Now, on to the content!

## Networking

* Looking for a step-by-step install guide for VMware NSX? [This one][link-1] is a bit older (refers to NSX 6.1 and the current release is 6.2), but you might find it helpful nevertheless.
* Ray Budavari---who is an absolutely _fantastic_ NSX resource---has [a blog post up on the integration between VMware NSX and vRealize Automation][link-2]. This first post (judging by the "part 1" in the title this will be a series) is a bit light on details, but I'm hoping future posts will dive deeper into this topic.
* Mustafa Akin has [an article on Docker's new overlay networking functionality][link-7]. He describes how to set it up (on both VirtualBox and Digital Ocean) and provides some high-level performance information as well.
* If you'd like to play around with Cumulus Linux but don't have a compatible hardware switch, Cumulus VX is the answer. Here's [a post on converting VX from VirtualBox to Libvirt][link-8], in case you'd like to run it that way.
* I saw [this announcement regarding a recent Juniper MX router refresh on SDx Central][link-13], but it is really light on details. It says the the JET (Juniper Extension Toolkit) uses "open application programming interfaces", but fails to provide any specifics. Is this "open-washing", or is there actually some meat to this? A general web search for "Juniper Extension Toolkit" yielded similarly low-value hits that were vague. If you have some details on this, [hit me up on Twitter][link-12].
* Here's a post that claims to [translate OVS and OpenStack Neutron to the network engineer's language][link-19]. One small point of discussion: the post claims OpenStack Neutron "isn't capable of controlling a physical network infrastructure" (the author's words). I would respond to that by saying OpenStack Neutron wasn't built to manage a physical network.

## Servers/Hardware

* Normally the hardware space is pretty boring (in fact, I've been considering removing it from the Technology Short Take series), but HPE decided to shake things up recently with its Synergy servers and "composable architecture". Most of the articles I found on the Synergy servers and "composable architecture" were more "market-techture" than anything substantive (see the list of links at the end of [this Mirantis blog post][link-11]), but as far as I'm able to glean this sounds a lot like Intel's Rack-Scale Architecture. (See [here][xref-1] for some of my thoughts on Intel's RSA following IDF 2014.) If HPE's Synergy takes the approach of enabling higher-level software to be more insightful and more effective, then great; if, on the other hand, HPE takes the approach of trying to replicate higher-level software functionality in hardware (as it seems they're trying to do), I'm not a fan of the added complexity.

## Security

* This article listing [20 Linux server hardening tips][link-4] contains some basic tips but is nevertheless a very good resource for someone looking for Linux security recommendations.

## Cloud Computing/Cloud Management

* Microsoft recently [announced a preview][link-5] of Azure Container Service (ACS). ACS offers multiple "endpoints," each of which enables you to use a particular open source container/orchestration tool. For example, there is a Docker Swarm endpoint, against which you could use standard Docker tools (like Docker Compose, for example).
* Rackspace and VMware have a pair of articles discussing their interoperable OpenStack cloud architecture ([here's the post from Rackspace][link-15], and [here's the post from VMware][link-14]). In my opinion, this is the sort of interoperability across providers and implementations that OpenStack really needs, so it's good to see two well-known names stepping up to make this happen. It would be great to see this expand to even more OpenStack providers, but it has to start somewhere, right?
* Trevor Roberts Jr. has a three-part series on using Vagrant with OpenStack, something [I've tackled on my site][xref-2] as well. Check out Trevor's posts ([part 1][link-16], [part 2][link-17], and [part 3][link-18]).
* I've been reviewing AWS VPC design recommendations recently, and one of the suggestions that comes up is using a VPC with a private subnet so that the instances on that VPC are not reachable from the Internet. Makes sense; if an instance isn't serving traffic from the Internet, then it shouldn't be reachable from the Internet. This, however, presents an issue; how do you provide _outbound_ Internet access to these instances? You can use a pair of NAT instances (this has scale limitations and adds complexity), you can use [the new Managed NAT Gateway][link-29], or you [can leverage a Squid proxy][link-30] (or even an AutoScaling Group of Squid proxies, if you're ambitious enough).
* This is an older post on [OpenStack availability zones and host aggregates][link-33], but useful nevertheless (for me, at least).

## Operating Systems/Applications

* Want to run Docker Swarm on Azure? Look [here][link-3].
* There's been a fair amount of noise regarding the Open Container Initiative recently, including a pair of blog posts from (somewhat) opposing viewpoints ([a post from Docker][link-9] and [a post from CoreOS][link-10]). Depending on your viewpoint, OCI is either the greatest thing since sliced bread, or it's a work in progress with a lot of potential. Time will tell which viewpoint was the most accurate.
* Splunk recently [announced][link-23] their Splunk Logging Driver for Docker, which allows Docker containers to send log data directly to Splunk. This comes on the heels of [the Docker announcement around their Ecosystem Technology Partner (ETP) program][link-24], which initially includes a whole list of logging-related partners (but didn't include Splunk, oddly enough). If you're interested in trying the Splunk driver, you'll need to use the Docker experimental build.
* [This][link-26] looks handy. (CLI tricks are so much fun.)
* I really appreciated Kelsey Hightower's [recent "12 Fractured Apps" article][link-27], in which he tackles some less-than-ideal application patterns with Docker containers. I'm not an application developer, but some of the suggestions Kelsey makes---like creating any directories the application needs if they don't exist---seem like ordinary common sense, and so part of me is surprised (although I shouldn't be) that this apparently isn't common practice.
* The recent release of CentOS 7.2 has caused some issues building Docker containers; see [this article][link-28] for a fix. (Thanks to Shannon McFarland for posting this on Twitter.)
* [Kubernetes cheat sheet?][link-32] Why, yes, thanks.

## Storage

* Joseph Griffiths has an article talking about an error condition he experienced where his [VMware hosts lost access to a volume when connected via Brocade SAN switches][link-6]. The fix, as Joseph describes it, is to be sure to use the correct fillword setting. It's been ages (OK, a few years) since I worked with Fibre Channel SAN switches, so this doesn't mean a whole lot to me---but hopefully it's helpful to someone out there.

## Virtualization

* William Lam [breaks down the real value of load balancing your PSC][link-20] in this in-depth article. Good write-up.
* I saw [this announcement][link-21] from Eric Sloof regarding _VMware vSphere PowerCLI Reference: Automating vSphere Administration, 2nd Edition_. I'm really glad to see that Wiley/Sybex worked with the authors to do a second edition of this massively helpful book. Congrats to the authors for all their hard work!
* Speaking of PowerCLI and good reference information: check out this article by Alan Renouf for [a reference guide to vSphere IDs][link-22].
* And while we are on a bit of a PowerCLI theme, have I mentioned [the PowerCLI-Example-Scripts GitHub repository][link-25]?

## Career/Soft Skills/Productivity

* Tom Hollingsworth recently weighed in on the topic of full-stack engineers in [his post titled "A Stack Full of It"][link-31]. I was in part of the discussion at ONUG that (apparently) triggered Tom's post. Tom makes some valid points, but I do respectfully disagree. I think full-stack engineers---folks that can work within and across multiple silos and layers of the data center---are a step in the _right_ direction. I'll have more to say on this topic very soon, so stay tuned.

I had more stuff to share with you, but I constrained myself to publish this last Short Take of 2015 no later than today, so I'll stop here. I hope that you found something useful!



[link-1]: http://dailyhypervisor.com/vmware-nsx-for-vsphere-6-1-step-by-step-installation/
[link-2]: https://blogs.vmware.com/networkvirtualization/2015/12/vmware-nsx-vrealize-automation.html
[link-3]: https://ahmetalpbalkan.com/blog/docker-swarm-azure/
[link-4]: http://www.cyberciti.biz/tips/linux-security.html
[link-5]: https://azure.microsoft.com/en-us/blog/azure-container-service-preview/
[link-6]: http://blog.jgriffiths.org/?p=620
[link-7]: http://mustafaak.in/2015/12/05/docker-overlay-performance.html
[link-8]: https://community.cumulusnetworks.com/cumulus/topics/converting-cumulus-vx-virtualbox-vagrant-box-gt-libvirt-vagrant-box
[link-9]: https://blog.docker.com/2015/12/progress-report-open-container-initiative/
[link-10]: https://coreos.com/blog/making-sense-of-standards/
[link-11]: https://www.mirantis.com/blog/hpe-introduces-a-new-way-of-computing-but-will-it-work/
[link-12]: https://twitter.com/scott_lowe
[link-13]: https://www.sdxcentral.com/articles/news/juniper-mx-routers-lean-toward-more-automation/2015/12/
[link-14]: http://blogs.vmware.com/openstack/vmware-rackspace-interoperable-openstack-architecture/
[link-15]: http://blog.rackspace.com/rackspace-vmware-interoperable-openstack-cloud-architecture/
[link-16]: http://blogs.vmware.com/openstack/vagrant-up-with-vmware-integrated-openstack-part-1/
[link-17]: http://blogs.vmware.com/openstack/vagrant-up-with-vmware-integrated-openstack-part-2/
[link-18]: https://blogs.vmware.com/openstack/vagrant-up-with-vmware-integrated-openstack-part-3/
[link-19]: http://cisqueros.blogspot.co.ke/2015/12/openstack-neutron-and-ovs-open-virtual.html
[link-20]: http://www.virtuallyghetto.com/2015/12/what-does-load-balancing-the-platform-services-controller-really-give-you.html
[link-21]: http://www.ntpro.nl/blog/archives/3021-New-Book-VMware-vSphere-PowerCLI-Reference-Automating-vSphere-Administration-2nd-Edition.html
[link-22]: http://www.virtu-al.net/2015/12/04/a-quick-reference-of-vsphere-ids/
[link-23]: http://blogs.splunk.com/2015/12/16/splunk-logging-driver-for-docker/
[link-24]: https://www.docker.com/press-release-12152015docker-unveils-new-ecosystem-technology-partners-log-management
[link-25]: https://github.com/vmware/PowerCLI-Example-Scripts
[link-26]: http://www.commandlinefu.com/commands/view/15049/get-ip-of-all-running-docker-containers
[link-27]: https://medium.com/@kelseyhightower/12-fractured-apps-1080c73d481c#.3s663gn8q
[link-28]: http://seven.centos.org/2015/12/fixing-centos-7-systemd-conflicts-with-docker/
[link-29]: https://aws.amazon.com/blogs/aws/new-managed-nat-network-address-translation-gateway-for-aws/
[link-30]: http://aws.amazon.com/articles/5995712515781075
[link-31]: http://networkingnerd.net/2015/11/18/a-stack-full-of-it/
[link-32]: http://k8s.info/cs.html
[link-33]: http://blog.russellbryant.net/2013/05/21/availability-zones-and-host-aggregates-in-openstack-compute-nova/
[xref-1]: {{< relref "2014-09-22-thinking-about-intel-rack-scale-architecture.md" >}}
[xref-2]: {{< relref "2015-09-28-using-vagrant-with-openstack.md" >}}
